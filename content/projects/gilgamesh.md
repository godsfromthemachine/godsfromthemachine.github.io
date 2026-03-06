---
title: "Gilgamesh"
description: "TDD-driven local AI coding agent"
---

<div class="card-grid" style="margin-bottom: 2rem;">
<div style="display: flex; gap: 0.75rem; align-items: center;">
  <span class="card-lang lang-go" style="font-size: 0.85rem; padding: 0.25rem 0.75rem;">Go</span>
  <span class="card-status status-active" style="font-size: 0.8rem;">ACTIVE &mdash; v0.5.0</span>
  <a href="https://github.com/godsfromthemachine/gilgamesh" class="btn btn-outline" style="padding: 0.3rem 0.8rem; font-size: 0.75rem;">GitHub</a>
</div>
</div>

Gilgamesh is an interactive CLI agent that connects to a local llama.cpp server (or any OpenAI-compatible endpoint) and provides tool-calling capabilities for software engineering tasks. It promotes a **test-driven development** approach and is designed for **CPU inference** with small models by keeping total prompt overhead under ~1,600 tokens.

## Features

- **7 built-in tools**: read, write, edit, bash, grep, glob, test
- **Multi-language testing**: auto-detects Go, Python, Rust, Zig, Node.js projects
- **Configurable tool permissions**: whitelist/blacklist tools per project
- **Streaming SSE**: tokens stream to terminal as they arrive
- **Multi-model profiles**: switch between fast/default/heavy models mid-session
- **Skills system**: reusable prompt templates (`.gilgamesh/skills/*.md`)
- **Hook system**: pre/post tool execution hooks (`.gilgamesh/hooks.json`)
- **Session logging**: JSONL session logs with distill summaries
- **Loop detection**: detects and breaks out of repeated tool calls
- **Context compaction**: automatically trims old tool results to stay within context limits
- **6 built-in skills**: commit, review, explain, fix, refactor, doc (embedded in binary)
- **Shell completion**: bash, zsh, fish (`gilgamesh completion bash`)
- **TDD-first**: system prompt promotes writing tests before implementation

## The CLI / MCP / API Duality

Gilgamesh embodies the core principle of Gods from the Machine: every capability is exposed through **three interfaces**.

<img src="/img/architecture.svg" alt="CLI / MCP / API Architecture" class="arch-diagram">

### CLI &mdash; For Humans

Interactive terminal REPL with slash commands, streaming output, and model switching.

```bash
./gilgamesh                              # interactive mode
./gilgamesh run "refactor this function" # one-shot mode
./gilgamesh -m heavy run "complex task"  # use heavy model
```

### MCP Server &mdash; For Agents

JSON-RPC 2.0 over stdio, compatible with Claude Desktop and any MCP client.

```bash
./gilgamesh mcp
```

```json
{
  "mcpServers": {
    "gilgamesh": {
      "command": "/path/to/gilgamesh",
      "args": ["mcp"]
    }
  }
}
```

### HTTP API &mdash; For Programs

REST endpoints with SSE streaming for the chat interface.

```bash
./gilgamesh serve -p 7777
```

| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/health` | Health check |
| GET | `/api/tools` | List all tools with schemas |
| POST | `/api/tools/{name}` | Execute a tool (JSON body = args) |
| POST | `/api/chat` | Agent conversation (SSE streaming) |

## Architecture

```
gilgamesh/
├── main.go           # CLI entry, REPL, subcommand dispatch
├── agent/            # Core agent loop + event-based variant
├── llm/              # OpenAI-compatible streaming SSE client
├── tools/            # Tool registration, dispatch, 7 built-in tools
├── mcp/              # JSON-RPC 2.0 MCP server
├── server/           # HTTP API server
├── config/           # JSON config loader (model profiles)
├── context/          # Project context + skills loader
├── hooks/            # Pre/post tool execution hooks
├── session/          # JSONL session logging + distill
└── cmd/bench/        # Go model benchmark tool
```

## Token Budget

The critical constraint for CPU inference:

| Component | Tokens |
|-----------|--------|
| System prompt | ~300 |
| 7 tool definitions | ~800 |
| Project context | ~500 (capped) |
| **Total overhead** | **~1,600** |

At 181 tok/s prompt processing (Qwen3.5-2B Q4_K_M, 16 threads), the first response arrives in ~10 seconds on CPU.

## Benchmarking &amp; Model Trials

Gilgamesh includes a pure Go benchmark tool for trialing local models:

```bash
go run ./cmd/bench              # benchmark default endpoint
go run ./cmd/bench -all         # benchmark all reachable endpoints
go run ./cmd/bench -model heavy # benchmark specific profile
```

Measures health latency, prompt speed (TTFT + tok/s), tool call parsing, one-shot agent response, and full edit task quality. Results are tracked in [`TRIALS.md`](https://github.com/godsfromthemachine/gilgamesh/blob/main/TRIALS.md).

### Key Findings

| Model | PP (tok/s) | TG (tok/s) | First Response | Verdict |
|-------|-----------|-----------|---------------|---------|
| Qwen3.5-2B Q4_K_M | 181 | 19 | ~7s | **Sweet spot** &mdash; default |
| Qwen3.5-4B Q8_0 | 54 | 6.3 | ~25s | Quality ceiling &mdash; heavy |
| Qwen3.5-0.8B | &mdash; | &mdash; | &mdash; | Rejected &mdash; too unreliable |

## Quick Start

```bash
# Build
git clone https://github.com/godsfromthemachine/gilgamesh
cd gilgamesh && go build -o gilgamesh .

# Configure (create gilgamesh.json)
cat > gilgamesh.json << 'EOF'
{
  "models": {
    "default": {
      "name": "qwen3.5-2b",
      "endpoint": "http://127.0.0.1:8081/v1",
      "api_key": "sk-local"
    }
  },
  "active_model": "default"
}
EOF

# Run
./gilgamesh
```
