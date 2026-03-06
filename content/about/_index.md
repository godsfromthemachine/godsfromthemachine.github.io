---
title: "About"
description: "The vision, motivation, and values behind Gods from the Machine"
---

## The Vision

We build autonomous, AI-powered software &mdash; "gods in the machine" &mdash; that operate entirely on local, private, offline AI models. No cloud dependencies. No API keys to external services. Just raw local inference powering intelligent programs.

Every project is designed to run on consumer hardware using small, capable models (Qwen 3.5 family: 0.8B, 2B, 4B, 9B) served via [llama.cpp](https://github.com/ggml-org/llama.cpp).

## Origins

> Inspired by the [Gods from the Machine](https://starwars.fandom.com/wiki/Machine_Gods_crisis) raid in Star Wars: The Old Republic &mdash; a pantheon of ancient, sentient super-weapons worshiped as deities on the mechanical Dyson sphere planet Iokath.

The metaphor is deliberate. In the SWTOR lore, the machine gods of Iokath were autonomous intelligences &mdash; each one specialized, each one powerful, each one operating independently yet part of a greater system. That's exactly what we're building: a pantheon of specialized AI-powered programs that can operate autonomously and communicate with each other.

## Why Local AI?

The AI revolution has been dominated by cloud APIs. You pay per token, your data goes to someone else's servers, and you're dependent on someone else's infrastructure. We believe there's a better way.

**Small language models have gotten remarkably capable.** A 2B parameter model quantized to 4-bit can:
- Write and debug code
- Call tools and navigate codebases
- Follow complex multi-step instructions
- Run at 19 tokens/second on a CPU

That's fast enough for interactive use. No GPU required.

**The tradeoff is token budget.** On CPU, every token in the system prompt delays the first response. Most existing AI tools use 10,000-40,000 token system prompts &mdash; completely unusable on local hardware. We keep gilgamesh's total overhead under 1,600 tokens. First response in ~10 seconds.

## The Hardware

Our target is consumer hardware. Specifically:

- **CPU**: AMD EPYC-Rome 16-core @ 2.0GHz (or equivalent)
- **RAM**: 30GB
- **GPU**: None
- **Inference**: llama.cpp with AVX2 (16 threads)

If it runs on this, it runs anywhere.

## Design Values

### Local-first
All intelligence comes from models running on your machine. Your code never leaves your hardware. No accounts, no billing, no rate limits.

### Zero Dependencies
Gilgamesh uses Go stdlib only. No third-party packages. This keeps the binary small (~9.8MB), builds instantly, and eliminates supply chain risk.

### Lean Token Budget
The #1 constraint for local CPU inference. We obsess over keeping system prompt and tool definition overhead minimal. ~1,600 tokens total &mdash; not 16,000.

### CLI + MCP + API
Every capability is exposed through three interfaces. Humans use the CLI. Other AI agents use MCP. Programs use HTTP. Same tools, same registry, different transport.

### Test-Driven Development
Gilgamesh promotes TDD in its system prompt and provides a dedicated `test` tool. It practices what it preaches with its own comprehensive test suite.

### High-Performance Languages
Go, Rust, Zig. No interpreted languages. Every millisecond matters when you're doing CPU inference.

## Open Source

All projects are MIT licensed. Browse the [GitHub organization](https://github.com/godsfromthemachine), open issues, submit PRs. We're building the future of autonomous local AI software.

## Author

Built by [Baalateja Kataru](https://github.com/bkataru).
