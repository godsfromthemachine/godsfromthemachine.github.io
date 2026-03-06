---
title: "Raijin"
description: "Lightning-fast CPU inference via ONNX runtime"
---

<div style="display: flex; gap: 0.75rem; align-items: center; margin-bottom: 2rem;">
  <span class="card-lang lang-rust" style="font-size: 0.85rem; padding: 0.25rem 0.75rem;">Rust</span>
  <span class="card-status status-complete" style="font-size: 0.8rem;">COMPLETE</span>
  <a href="https://github.com/godsfromthemachine/raijin" class="btn btn-outline" style="padding: 0.3rem 0.8rem; font-size: 0.75rem;">GitHub</a>
</div>

Raijin is a lightning-fast CPU inference engine built in Rust using the ONNX runtime. Named after the Japanese god of thunder and lightning, it delivers blazing-fast local model execution.

## Features

- **ONNX runtime**: leverages Microsoft's ONNX Runtime for optimized CPU inference
- **Pure Rust**: safe, fast, memory-efficient
- **Local-first**: all inference happens on your machine
- **Cross-platform**: runs anywhere Rust and ONNX runtime compile

## Why ONNX?

While llama.cpp excels at GGUF-based transformer inference, ONNX runtime provides a complementary path for models exported in the ONNX format. This is particularly useful for smaller, task-specific models (classification, embedding, summarization) that don't need the full generative pipeline.

## Status

Raijin is complete and functional. It serves as the ONNX inference backend for projects that need fast, lightweight model execution outside the llama.cpp ecosystem.
