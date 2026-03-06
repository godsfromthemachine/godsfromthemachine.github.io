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
  <div class="phase-dot active"></div>
  <div class="phase-title">Phase 4: Gilgamesh v0.3 &mdash; Quality & Polish <span class="phase-status" style="color: var(--cyan);">IN PROGRESS</span></div>
  <ul class="phase-items">
    <li class="done">Unit tests for tools package (registry, read, glob, grep)</li>
    <li class="done">Unit tests for MCP server (initialize, tools/list, tools/call)</li>
    <li class="done">Unit tests for HTTP server (health, tools, error handling)</li>
    <li class="done">Unit tests for config, context, hooks, and session packages</li>
    <li class="done">CI/CD with GitHub Actions (build + test on push/PR)</li>
    <li class="todo">Table-driven tests with edge cases for tools</li>
    <li class="todo">Better error messages and diagnostics</li>
    <li class="todo">Graceful shutdown for HTTP server</li>
    <li class="todo">Request timeout middleware</li>
    <li class="todo">MCP protocol version negotiation</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot future"></div>
  <div class="phase-title">Phase 5: Gilgamesh v1.0 &mdash; Production Ready <span class="phase-status">PLANNED</span></div>
  <ul class="phase-items">
    <li class="todo">Memory/context persistence across sessions</li>
    <li class="todo">Project-scoped conversation history</li>
    <li class="todo">Configurable tool permissions</li>
    <li class="todo">Custom tool registration (user-defined)</li>
    <li class="todo">Multi-language test support</li>
    <li class="todo">TDD workflow automation (red-green-refactor)</li>
    <li class="todo">Shell completion (bash, zsh, fish)</li>
  </ul>
</div>

<div class="phase">
  <div class="phase-dot future"></div>
  <div class="phase-title">Phase 6: Broader Gods <span class="phase-status">FUTURE</span></div>
  <ul class="phase-items">
    <li class="todo">Zeus: complete Zig build system for llama.cpp</li>
    <li class="todo">New god projects: security tools, agentic browsers</li>
    <li class="todo">Cross-god integration via MCP/HTTP</li>
    <li class="todo">Shared tool library across gods</li>
    <li class="todo">Publishing and packaging (GitHub releases, homebrew)</li>
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
