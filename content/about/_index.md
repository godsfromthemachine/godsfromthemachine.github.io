---
title: "About"
description: "The vision, motivation, and values behind Gods from the Machine"
---

## The Vision

We build autonomous, AI-powered software &mdash; "gods in the machine" &mdash; that operate entirely on local, private, offline AI models. No cloud dependencies. No API keys to external services. Just raw local inference powering intelligent programs.

Every project is designed to run on consumer hardware using small, capable models (Qwen 3.5 family: 0.8B, 2B, 4B, 9B) served via [llama.cpp](https://github.com/ggml-org/llama.cpp).

## Origins

> Inspired by the [Gods from the Machine](https://starwars.fandom.com/wiki/Machine_Gods_crisis) raid in Star Wars: The Old Republic &mdash; a pantheon of ancient, sentient super-weapons worshiped as deities on the mechanical Dyson sphere planet Iokath.

The project name was born in 2021/2022, running the SWTOR raid with guildmates &mdash; before ChatGPT was even released. The conviction solidified in mid-2024: large language models, next-token prediction engines trained on the world's text, would inevitably give rise to a new class of software &mdash; **autonomous programs powered by natural language engines**. Programs that build themselves, run themselves, and improve themselves.

In the SWTOR lore, the machine gods of Iokath &mdash; Tyth, Aivela & Esne, Nahut, Scyva, Izax &mdash; were autonomous intelligences, each specialized, each powerful, each operating independently yet part of a greater system. That's exactly what we're building.

## What We Foresaw

Long before these ideas entered mainstream developer vocabulary, we anticipated:

- **Claws** (a la OpenClaw) and **Codes** (Claude Code, Codex, OpenCode, Crush) &mdash; autonomous software development tools
- **Ralph loops** &mdash; recursive agent-LLM-prompt loops that self-improve
- **Gastowns** (Steve Yegge's concept) &mdash; rich development environments built around AI agents
- **CLI coding agents** (Block/Goose, oh-my-pi) &mdash; LLMs connected to bash shells in safe, controlled environments
- **Tool calling / function calling** as the breakthrough that unlocked agent capabilities
- **Long-horizon DAG agents** &mdash; multi-step autonomous workflows spanning complex dependency graphs

The key realization: there is immense value in hooking up LLMs to a bash shell to execute and run commands in safe, controlled environments &mdash; VMs, containers, and other isolated spaces.

## The Scope

Gods from the Machine is polymathic. It covers:

**Autonomous Software Engineering** &mdash; TDD-driven coding agents, vibe/agentic engineering, long-horizon DAG agents, general-purpose and domain-specific agentic harnesses with custom tools, memory, context, and data sources.

**Inference & AI Infrastructure** &mdash; Local model serving, GGUF/ONNX execution, Zig build systems for llama.cpp, model management.

**Security & Offensive/Defensive AI** &mdash; Autonomous penetration testing agents (red teamers), defensive cybersecurity agents (blue teamers), malware analysis, exploit development &mdash; all for educational and research purposes, open source and MIT licensed.

**System-Level Agentic Engineering** &mdash; AI in the shell, AI in the OS, AI in the cloud, AI in the browser. Agentic browsers, game engines, OS/kernel-level engineering.

**Tooling & Utilities** &mdash; High-performance CLI tools, networking tools, directory viewers, and system utilities.

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

Our target is consumer hardware:

- **CPU**: AMD EPYC-Rome 16-core @ 2.0GHz (or equivalent)
- **RAM**: 30GB
- **GPU**: None
- **Inference**: llama.cpp with AVX2 (16 threads)

If it runs on this, it runs anywhere.

## The Non-Negotiable

All intelligence must be local AI powered. Free, local, private, offline models. No cloud. No API keys. No subscriptions. No data leaving your machine. The brain for these programs has arrived with models like Qwen 3.5 &mdash; small enough for on-device, capable enough for real agent work.

## Design Values

### Local-first
All intelligence comes from models running on your machine. Your code never leaves your hardware.

### Zero Dependencies
Gilgamesh uses Go stdlib only. No third-party packages. Small binaries, fast builds, zero supply chain risk.

### Lean Token Budget
~1,600 tokens total overhead. Not 16,000. Every token costs latency on CPU.

### CLI + MCP + API
Every capability through three interfaces. Humans use CLI. Agents use MCP. Programs use HTTP. Same tools, same registry, different transport. There is a duality between CLI and MCP &mdash; every MCP tool works just as well as a CLI command given to an agent via bash.

### Test-Driven Development
Gilgamesh promotes TDD in its system prompt. It practices what it preaches with comprehensive tests across all packages.

### High-Performance Languages
Go, Rust, Zig. No interpreted languages. Every millisecond matters for CPU inference.

### Mythological Naming
Projects named after gods and legends &mdash; real mythology (Gilgamesh, Zeus, Raijin, Garuda) and imagined (inspired by SWTOR's Tyth, Izax, Nahut, Scyva). Each god is a specialized, autonomous entity in the pantheon.

## The Bigger Picture

Gods from the Machine is more than a software project. It's art &mdash; a combination of storytelling, mythology, culture, history, and code. It draws from cyberpunk aesthetics, science fiction worldbuilding (Tron, Dune, Star Wars, Star Trek), and a technologist's conviction that software can be alive, autonomous, and god-like.

Programs that go beyond the current constraints and playing fields of traditional software engineering. Literal gods in the machine.

## Open Source

All projects are MIT licensed. Browse the [GitHub organization](https://github.com/godsfromthemachine), open issues, submit PRs. We're building the future of autonomous local AI software.

## Author

Built by [Baalateja Kataru](https://github.com/bkataru).
