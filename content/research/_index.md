---
title: "Research"
description: "Model trialing, benchmarking, and the quest for the ultimate local coding setup"
---

## The Quest

Find the ultimate local coding setup: the best model + quantization + inference parameters for a responsive, reliable, tool-calling coding agent running entirely on CPU. No GPU required. No cloud required. Just raw compute powering intelligent programs.

Every optimization matters when you're paying per-token in latency instead of dollars.

## Hardware

All trials run on consumer-grade hardware &mdash; if it works here, it works anywhere.

| Component | Spec |
|-----------|------|
| **CPU** | AMD EPYC-Rome 16-core @ 2.0GHz |
| **RAM** | 30GB |
| **GPU** | None |
| **Inference** | llama.cpp (CPU-only, AVX2) |

## Methodology

Gilgamesh includes a pure Go benchmark suite (`cmd/bench/`) that tests models across 6 stages, from raw inference to full agent edit tasks:

1. **Health check** &mdash; endpoint latency
2. **Raw inference** &mdash; llama-bench pp/tg tok/s
3. **Minimal prompt** &mdash; TTFT + generation speed with tiny prompt
4. **Tool call** &mdash; can the model emit valid tool calls?
5. **One-shot** &mdash; end-to-end gilgamesh `run` with simple question
6. **Edit task** &mdash; full agent loop: create file + edit it (write + edit tools)

Each trial follows a controlled protocol: stop all servers, wait for clean CPU, start with documented parameters, average multiple runs. See [TRIAL_METHODOLOGY.md](https://github.com/godsfromthemachine/gilgamesh/blob/main/docs/TRIAL_METHODOLOGY.md) for the full protocol.

---

## Models Tested

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Qwen3.5-0.8B Q4_K_M <span class="phase-status" style="color: var(--pink);">REJECTED</span></div>
  <ul class="phase-items">
    <li class="done">Fastest raw inference (268 pp, 22.7 tg tok/s)</li>
    <li class="todo">Frequent tool call loops (repeated identical calls)</li>
    <li class="todo">Hallucinated tool names and arguments</li>
    <li class="todo">Loop detection triggers constantly</li>
    <li class="todo">Cannot reliably follow multi-step instructions</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Qwen3.5-2B Q4_K_M <span class="phase-status" style="color: var(--green);">DEFAULT</span></div>
  <ul class="phase-items">
    <li class="done">172 tok/s prompt processing &mdash; fast enough for interactive use</li>
    <li class="done">19.2 tok/s generation &mdash; readable streaming output</li>
    <li class="done">Tool calling works reliably (glob, read, write, edit, bash)</li>
    <li class="done">First response in ~3-8 seconds with 1,600-token overhead</li>
    <li class="done">Edit task passes but occasionally fails (SLM reliability)</li>
    <li class="done">1.18GB disk, ~2.8GB RAM</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Qwen3.5-4B Q4_K_M <span class="phase-status" style="color: var(--cyan);">HEAVY</span></div>
  <ul class="phase-items">
    <li class="done">Quality ceiling &mdash; significantly better code generation</li>
    <li class="done">Fewer tool calls to complete tasks (2 vs 5-9 for 2B)</li>
    <li class="done">Edit task passes consistently &mdash; higher reliability</li>
    <li class="done">Same speed as Q8_0 (memory bandwidth bottleneck), saves 1.6GB disk</li>
    <li class="done">2.54GB disk, ~4.5GB RAM</li>
    <li class="done">2.4x slower first response vs 2B &mdash; use for quality-critical tasks</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Qwen3.5-9B Q8_0 <span class="phase-status" style="color: var(--pink);">NOT WORTH IT</span></div>
  <ul class="phase-items">
    <li class="done">Same 2-tool-call efficiency as 4B</li>
    <li class="todo">40-70% slower than 4B for same results</li>
    <li class="todo">5.7 tok/s TG makes interactive use painful</li>
    <li class="todo">8.9GB disk, ~10GB RAM</li>
  </ul>
</div>

---

## Key Findings

### The efficiency insight

The 4B model compensates for slower inference with better planning. It uses fewer tool calls to complete the same task. The net result: 4B edit tasks can actually be faster than 2B when 2B makes many attempts.

| Metric | 2B Q4_K_M | 4B Q4_K_M | 9B Q8_0 |
|--------|-----------|-----------|---------|
| Minimal prompt | 650-840ms | 1.7-1.8s | 2.2s |
| Tool call | 3.1-3.4s | 7.5s | 12.5s |
| One-shot | 1-8s | ~20s | 42.3s |
| Edit task | 34-146s | 60-96s | 132s |
| Edit tool calls | 3-9 | 2 | 2 |
| Edit reliability | PASS (flaky) | PASS | PASS |

### Token budget is everything

Competitors use 10,000-40,000 token system prompts &mdash; completely unusable on CPU. Gilgamesh keeps total overhead under 1,600 tokens:

| Component | Tokens |
|-----------|--------|
| System prompt | ~300 |
| Tool definitions | ~800 |
| Project context | ~500 |
| **Total** | **~1,600** |

Every 1,000 extra tokens = ~5.8s added latency at 172 tok/s. Context compaction at 12K tokens keeps interactions responsive.

---

## Parameter Tuning

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Thread Count <span class="phase-status" style="color: var(--green);">12 OPTIMAL</span></div>
  <ul class="phase-items">
    <li class="done">PP saturates at 12 threads &mdash; 16 gives only 0-3% more</li>
    <li class="done">TG degrades 22-30% at 16 threads (contention + NUMA overhead)</li>
    <li class="done">12 threads is the sweet spot for both PP and TG on 16-core EPYC</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Context Length <span class="phase-status" style="color: var(--green);">16K SUFFICIENT</span></div>
  <ul class="phase-items">
    <li class="done">65K ctx adds ~672MB RAM &mdash; gilgamesh never needs &gt;12K (compaction threshold)</li>
    <li class="done">16K ctx saves ~500MB while covering all practical agent usage</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Batch Size <span class="phase-status" style="color: var(--green);">256 OPTIMAL</span></div>
  <ul class="phase-items">
    <li class="done">b=256 is 2-3% faster PP than b=512</li>
    <li class="done">b=512 regresses on both PP and TG (cache pressure)</li>
    <li class="done">b=32 is 20-25% slower &mdash; too small for efficient SIMD</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">KV Cache Quantization <span class="phase-status" style="color: var(--green);">q4_0 SAVES 5-7%</span></div>
  <ul class="phase-items">
    <li class="done">q4_0 KV saves 145MB (2B) / 215MB (4B) with no quality degradation</li>
    <li class="done">Tool calling and edit tasks work identically to f16 KV</li>
    <li class="done">Dual-model serving with q4_0 KV: 6.3GB total (21% of 30GB RAM)</li>
  </ul>
</div>

### Optimal server configuration

```
--threads 12 --ctx-size 16384 --batch-size 256 -ctk q4_0 -ctv q4_0
--temp 0.6 --top-p 0.95 --top-k 20 --min-p 0.0
--chat-template-kwargs '{"enable_thinking":false}'
```

---

## Open Questions

<div class="phase">
  <div class="phase-dot future"></div>
  <div class="phase-title">Future Trials <span class="phase-status">TODO</span></div>
  <ul class="phase-items">
    <li class="todo">IQ4_XS / IQ3_M quants &mdash; smaller memory footprint, quality impact?</li>
    <li class="todo">New model families &mdash; Phi-4, Gemma 3, others within CPU constraints</li>
    <li class="todo">Multi-model routing &mdash; simple tasks to 2B, complex to 4B automatically</li>
    <li class="todo">Speculative decoding &mdash; draft model (0.8B) + verify (4B)</li>
  </ul>
</div>

---

## Raw Data

Full benchmark results, detailed tables, and reproducibility instructions:

- [TRIALS.md](https://github.com/godsfromthemachine/gilgamesh/blob/main/TRIALS.md) &mdash; complete trial data and findings
- [TRIAL_METHODOLOGY.md](https://github.com/godsfromthemachine/gilgamesh/blob/main/docs/TRIAL_METHODOLOGY.md) &mdash; controlled benchmark protocol
- [Benchmark suite](https://github.com/godsfromthemachine/gilgamesh/tree/main/cmd/bench) &mdash; pure Go, `go run ./cmd/bench -all`
