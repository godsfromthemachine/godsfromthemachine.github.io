---
title: "Zeus"
description: "Zig build system for llama.cpp GGUF inference"
---

<div style="display: flex; gap: 0.75rem; align-items: center; margin-bottom: 2rem;">
  <span class="card-lang lang-zig" style="font-size: 0.85rem; padding: 0.25rem 0.75rem;">Zig</span>
  <span class="card-status status-stub" style="font-size: 0.8rem;">INCOMPLETE</span>
  <a href="https://github.com/godsfromthemachine/zeus" class="btn btn-outline" style="padding: 0.3rem 0.8rem; font-size: 0.75rem;">GitHub</a>
</div>

Zeus is a Zig-as-a-build-system project for llama.cpp-powered local GGUF inference. It aims to provide a single-command workflow for building, quantizing, and serving local AI models.

## Vision

- **Single binary**: build llama.cpp from source with Zig's build system
- **Model management**: download, quantize, and organize GGUF models
- **Server orchestration**: launch and manage llama.cpp server instances
- **Zero dependencies**: Zig handles the entire C/C++ compilation toolchain

## Why Zig?

Zig's build system can compile C and C++ code natively, making it an ideal choice for building llama.cpp without requiring CMake, Make, or any other build tools. A single `zig build` replaces an entire toolchain.

## Status

Zeus is an incomplete, early-stage project. The repository exists with initial structure but the core functionality is not yet implemented. It is on the [roadmap](/roadmap/) for future development as part of Phase 6.
