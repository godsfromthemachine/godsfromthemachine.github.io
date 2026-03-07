---
title: "Roadmap"
description: "Project milestones, phases, and future plans"
---

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Phase 1: Foundation <span class="phase-status" style="color: var(--green);">COMPLETE</span></div>
  <ul class="phase-items">
    <li class="done">Create godsfromthemachine GitHub organization</li>
    <li class="done">Write vision document and project origins</li>
    <li class="done">Create org profile repo with landing page</li>
    <li class="done">Set up initial repos: gilgamesh, zeus, raijin, garuda</li>
    <li class="done">Generate branding assets and visual identity</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Phase 2: Gilgamesh v0.1 &mdash; Core Agent <span class="phase-status" style="color: var(--green);">COMPLETE</span></div>
  <ul class="phase-items">
    <li class="done">7 built-in tools: read, write, edit, bash, grep, glob, test</li>
    <li class="done">Streaming SSE client for OpenAI-compatible endpoints</li>
    <li class="done">Interactive CLI REPL with slash commands</li>
    <li class="done">One-shot mode (gilgamesh run "prompt")</li>
    <li class="done">Multi-model profiles (fast/default/heavy)</li>
    <li class="done">Skills system with argument substitution</li>
    <li class="done">Hook system for pre/post tool execution</li>
    <li class="done">Session logging (JSONL with distill)</li>
    <li class="done">Loop detection and context compaction</li>
    <li class="done">TDD-first system prompt</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Phase 3: Gilgamesh v0.2 &mdash; CLI/MCP/API Duality <span class="phase-status" style="color: var(--green);">COMPLETE</span></div>
  <ul class="phase-items">
    <li class="done">MCP server mode (gilgamesh mcp) &mdash; JSON-RPC 2.0 over stdio</li>
    <li class="done">HTTP API server mode (gilgamesh serve)</li>
    <li class="done">REST endpoints: /api/health, /api/tools, /api/tools/{name}, /api/chat</li>
    <li class="done">SSE streaming for /api/chat endpoint</li>
    <li class="done">RunWithEvents() agent variant for non-terminal consumers</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Phase 4: Gilgamesh v0.3 &mdash; Quality & Polish <span class="phase-status" style="color: var(--green);">COMPLETE</span></div>
  <ul class="phase-items">
    <li class="done">Unit tests for all 9 packages (agent, llm, tools, mcp, server, config, context, hooks, session)</li>
    <li class="done">CI/CD with GitHub Actions (build + test on push/PR)</li>
    <li class="done">Go benchmark tool (cmd/bench/) for model trialing</li>
    <li class="done">Model trials documentation (TRIALS.md)</li>
    <li class="done">Table-driven tests with edge cases for tools</li>
    <li class="done">Graceful shutdown for HTTP server with request timeouts</li>
    <li class="done">MCP protocol version negotiation (2025-03-26 + 2024-11-05)</li>
    <li class="done">Version bump to v0.3.0</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Phase 5: Gilgamesh v0.5 &mdash; Skills, Permissions &amp; Multi-language <span class="phase-status" style="color: var(--green);">COMPLETE</span></div>
  <ul class="phase-items">
    <li class="done">Configurable tool permissions (allowed_tools/denied_tools)</li>
    <li class="done">Multi-language test support (Go, Python, Rust, Zig, Node.js)</li>
    <li class="done">Shell completion (bash, zsh, fish)</li>
    <li class="done">7 built-in skills embedded in binary (commit, review, explain, fix, refactor, doc, tdd)</li>
    <li class="done">3-tier skill precedence: built-in &rarr; global &rarr; project-local</li>
    <li class="done">Version bump to v0.5.0</li>
    <li class="done">Memory/context persistence across sessions (.gilgamesh/memory.json)</li>
    <li class="done">Project-scoped conversation history (save/resume)</li>
    <li class="done">Custom tool registration (.gilgamesh/tools.json)</li>
    <li class="done">TDD workflow automation (/tdd built-in skill)</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot done"></div>
  <div class="phase-title">Phase 6: Gilgamesh v0.6 &mdash; UI &amp; Developer Experience <span class="phase-status" style="color: var(--green);">COMPLETE</span></div>
  <ul class="phase-items">
    <li class="done">Command registry pattern replacing switch statement</li>
    <li class="done">Graceful Ctrl+C via context.Context threading</li>
    <li class="done">Streaming markdown renderer (headers, bold/italic, lists, blockquotes, code blocks)</li>
    <li class="done">Accessible NoColor icon fallbacks</li>
    <li class="done">Auto-sized table renderer and context pressure gauge</li>
    <li class="done">Error classification with recovery hints</li>
    <li class="done">Config validation and environment variable overrides</li>
    <li class="done">Spinner with elapsed time display</li>
    <li class="done">Terminal color profile detection (NO_COLOR, CLICOLOR, COLORTERM)</li>
    <li class="done">243 tests across 12 packages</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot future"></div>
  <div class="phase-title">Phase 7: Gilgamesh v1.0 &mdash; Intelligence <span class="phase-status">FUTURE</span></div>
  <ul class="phase-items">
    <li class="todo">Multi-model routing &mdash; automatic 2B/4B selection based on task complexity</li>
    <li class="todo">Distill workflow &mdash; session &rarr; skill extraction</li>
    <li class="done">Trials methodology documentation (docs/TRIAL_METHODOLOGY.md)</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot future"></div>
  <div class="phase-title">Phase 8: Broader Gods <span class="phase-status">FUTURE</span></div>
  <ul class="phase-items">
    <li class="todo">Zeus: complete Zig build system for llama.cpp</li>
    <li class="todo">New god projects: security tools, agentic browsers</li>
    <li class="todo">Cross-god integration via MCP/HTTP</li>
    <li class="todo">Shared tool library across gods</li>
    <li class="todo">Publishing and packaging (GitHub releases, homebrew)</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot active"></div>
  <div class="phase-title">Running: Model Trialing &amp; Benchmarking <span class="phase-status" style="color: var(--cyan);">ONGOING</span></div>
  <ul class="phase-items">
    <li class="done">Go benchmark tool (cmd/bench/main.go)</li>
    <li class="done">Baseline benchmarks: Qwen3.5 2B Q4_K_M, 4B Q8_0</li>
    <li class="done">Key findings documented (2B sweet spot, 0.8B rejected)</li>
    <li class="done">Enhanced bench suite: config loading, raw llama-bench, JSON output, result persistence</li>
    <li class="done">4B Q4_K_M raw trial &mdash; same speed as Q8_0, saves 1.6GB disk</li>
    <li class="done">Full -all comparison run: 2B vs 4B with summary table</li>
    <li class="done">4B Q4_K_M agent bench &mdash; outperforms Q8_0, recommend as heavy profile</li>
    <li class="done">Thread tuning &mdash; 12 threads optimal on 16-core EPYC, 30% better TG</li>
    <li class="done">Context length impact &mdash; 16K ctx saves 672MB, sufficient for agent</li>
    <li class="done">Batch size tuning &mdash; b=256 optimal, b=512 regresses</li>
    <li class="done">9B Q8_0 agent benchmarks &mdash; not worth it, same efficiency as 4B but slower</li>
    <li class="done">KV cache quantization &mdash; q4_0 saves 5-7% RAM, no quality loss</li>
    <li class="todo">Trial IQ4_XS / IQ3_M quants &mdash; smaller memory footprint</li>
    <li class="todo">New model families &mdash; Phi-4, Gemma 3, others</li>
    <li class="todo">Speculative decoding &mdash; draft model (0.8B) + verify (4B)</li>
    <li class="todo">Multi-model routing &mdash; simple tasks &rarr; 2B, complex &rarr; 4B</li>
  </ul>
</div>

---

## Principles

Every milestone must satisfy:

1. **Local-first** &mdash; all intelligence from local AI models
2. **Zero external deps** &mdash; Go stdlib only for gilgamesh
3. **CLI + MCP + API** &mdash; every capability through all three interfaces
4. **Lean** &mdash; minimal token overhead for CPU inference
5. **TDD** &mdash; gilgamesh eats its own dogfood with comprehensive tests
