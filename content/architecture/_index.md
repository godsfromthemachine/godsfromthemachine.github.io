---
title: "Architecture"
description: "Technical architecture and design decisions"
---

## The Duality Principle

Every "god" in the pantheon exposes its capabilities through three transport layers. The **tools are the fundamental unit** &mdash; CLI, MCP, and HTTP are just different transports over the same tool registry.

A tool that reads a file works identically whether invoked by a human typing in the REPL, another AI agent via MCP, or a script via HTTP.

```
             Human users ──── CLI (terminal REPL)
                                    │
Other agents ──── MCP (JSON-RPC stdio) ──── Tool Registry ──── Filesystem
                                    │                          Shell
     Programs ──── HTTP (REST/SSE) ─┘                          Network
```

<img src="/img/architecture.svg" alt="CLI / MCP / API Architecture" class="arch-diagram">

## Why Three Interfaces?

| Interface | Transport | Consumer | Use Case |
|-----------|-----------|----------|----------|
| **CLI** | Terminal stdin/stdout | Humans | Interactive coding, debugging, exploration |
| **MCP** | JSON-RPC 2.0 over stdio | AI agents | Claude Desktop, other coding agents |
| **HTTP** | REST + SSE | Programs | Automation, CI/CD, web UIs, scripts |

The same tool registry powers all three. No capability is exclusive to any interface.

## Gilgamesh Architecture

### Package Map

```
gilgamesh/
├── main.go              Entry point, REPL, subcommand dispatch
├── agent/
│   ├── agent.go         Core loop: prompt → LLM → tool → repeat
│   │                    Run() for CLI, RunWithEvents() for HTTP
│   └── prompt.go        TDD-first system prompt (~300 tokens)
├── llm/
│   └── client.go        OpenAI-compatible streaming SSE client
├── tools/
│   ├── registry.go      Tool registration, dispatch, enumeration
│   ├── read.go          Read files (offset/limit, numbered lines)
│   ├── write.go         Create/overwrite files (auto-mkdir)
│   ├── edit.go          Find-and-replace (unique match required)
│   ├── bash.go          Shell execution (120s timeout, 10K cap)
│   ├── grep.go          Content search (regex, 50 match cap)
│   ├── glob.go          File pattern matching (100 file cap)
│   └── test.go          Multi-language test runner (Go, Python, Rust, Zig, Node)
├── mcp/
│   ├── protocol.go      JSON-RPC 2.0 + MCP types
│   └── server.go        Stdio MCP server
├── server/
│   └── server.go        HTTP API server
├── config/              JSON config loader (model profiles)
├── context/             Project context + skills loader
├── memory/              Project-scoped persistent memory
├── hooks/               Pre/post tool execution hooks
├── session/             JSONL session logging + conversation history
└── cmd/bench/           Go model benchmark tool
```

### Agent Loop

The core of gilgamesh is a loop that sends user input to a local LLM, processes tool calls, and feeds results back.

```
User Input
    │
    ▼
┌──────────┐
│  System   │  ~300 tokens base + project context
│  Prompt   │
└────┬─────┘
     │
     ▼
┌──────────┐     ┌──────────┐
│ StreamChat│────▶│  LLM     │  local llama.cpp / OpenAI-compatible
│ (SSE)    │◀────│  Server   │
└────┬─────┘     └──────────┘
     │
     ├── Text content → print to terminal / emit event
     │
     └── Tool calls → for each:
             │
             ├── Pre-hooks (can block)
             ├── Registry.Execute(name, args)
             ├── Post-hooks (observe)
             ├── Session log
             └── Append result → loop back to LLM
                 (max 15 iterations, loop detection)
```

### Token Budget

The critical constraint for CPU inference. Every token in the system prompt delays the first response.

| Component | Tokens |
|-----------|--------|
| System prompt | ~300 |
| 7 tool definitions | ~800 |
| Project context | ~500 (capped) |
| **Total overhead** | **~1,600** |
| Typical user message | 50-200 |
| **First request** | **~1,700-1,800** |

At ~160 tok/s prompt processing (Qwen3.5-2B Q4_K_M, 12 threads), the first response arrives in ~10 seconds. Subsequent turns benefit from KV cache.

### MCP Protocol Flow

```
Client                          Gilgamesh MCP Server
  │                                    │
  │──── initialize ───────────────────▶│
  │◀─── serverInfo + capabilities ─────│
  │                                    │
  │──── notifications/initialized ────▶│  (no response)
  │                                    │
  │──── tools/list ───────────────────▶│
  │◀─── 7 tools with inputSchema ─────│
  │                                    │
  │──── tools/call {name, args} ──────▶│
  │         │ pre-hooks → execute → post-hooks
  │◀─── {content: [{type:"text",...}]} │
```

### HTTP API Flow

```
GET  /api/health          → {"status":"ok","version":"0.5.0"}
GET  /api/tools           → [{name, description, parameters}, ...]
POST /api/tools/{name}    → {"result":"...", "elapsed":"42µs"}
POST /api/chat            → SSE stream of agent events:
                              data: {"type":"content","content":"..."}
                              data: {"type":"tool_call","tool":"read","args":{...}}
                              data: {"type":"tool_result","tool":"read","content":"..."}
                              data: {"type":"done"}
```

## Benchmarking Infrastructure

Gilgamesh includes a pure Go benchmark suite (`cmd/bench/`) for trialing local models. It loads profiles from `gilgamesh.json`, integrates with llama-bench for raw inference metrics, and supports JSON output for historical tracking.

It measures six dimensions:

1. **Health check** &mdash; endpoint latency
2. **Raw inference** &mdash; llama-bench pp/tg tok/s (auto-detects binaries in `local-ai/bin/`)
3. **Minimal prompt** &mdash; TTFT + generation speed via API
4. **Tool call** &mdash; can the model emit valid tool calls?
5. **One-shot** &mdash; end-to-end gilgamesh `run` response
6. **Edit task** &mdash; full agent loop: create file + edit it

Supports `-all` (compare all profiles), `-raw` (raw llama-bench), `-json` (machine-readable), `-save` (append to JSON log).

Results and ongoing findings are tracked in [`TRIALS.md`](https://github.com/godsfromthemachine/gilgamesh/blob/main/TRIALS.md). The quest: find the optimal model + quantization + inference parameters for a responsive, reliable, tool-calling agent running entirely on CPU.

## Design Decisions

### Zero External Dependencies

Gilgamesh uses Go stdlib only. No cobra, no glamour, no third-party HTTP frameworks. This keeps the binary small (~9.8MB), builds fast, and eliminates supply chain risk.

### Streaming-First

All LLM responses are streamed token-by-token via SSE. The agent loop processes tool calls as they arrive, providing perceived responsiveness even on slow CPU inference.

### Closure-Based Tools

Each tool is a closure capturing its logic in an `Execute func(args json.RawMessage) (string, error)` field. This keeps the tool registry generic &mdash; any function that takes JSON and returns a string can be a tool.

### Hooks for Extensibility

Instead of a plugin system (too complex, too many tokens), hooks run shell commands before/after tool execution. Users get full control over tool behavior without touching agent code.

### Skills as Prompt Templates

Skills are markdown files with `{{args}}` placeholders. They're injected as the user message, not as system prompt additions. This keeps the token overhead constant regardless of how many skills exist.

## Model Profiles

```json
{
  "fast":    { "name": "qwen3.5-2b", "endpoint": "http://127.0.0.1:8081/v1" },
  "default": { "name": "qwen3.5-2b", "endpoint": "http://127.0.0.1:8081/v1" },
  "heavy":   { "name": "qwen3.5-4b", "endpoint": "http://127.0.0.1:8080/v1" }
}
```

Switch models mid-session with `/model heavy` or specify on launch with `-m fast`.
