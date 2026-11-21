# META PROMPT: Code review + slerm/ extension plan for slermes

## AGENT IDENTITY

You are reviewing and extending the slermes repository at `/c/Users/eman5/slermes/slermes/`. This is a C project — a production-grade C translation of the Hermes AI agent. Your job is to understand exactly what exists, what Odysseus (a Python reference) does differently, and what specifically needs to be added to a new `slerm/` subfolder inside this repo.

**You are NOT rewriting the agent core.** That already exists and works. You are a plugin layer on top of it.

## WHO MADE WHAT

- **pewdiepie** (Felix Kjellberg) made **Odysseus** (`github.com/pewdiepie-archdaemon/odysseus`). Fork: `waefrebeorn/slermes-odyssey`.
- **you (waefrebeorn)** created **slermes** (this repo) and **bytropix** (a separate C+CUDA inference project, provided as your coding reference only — do not focus on it).
- `slermes-odyssey` is the only connection between you two. Do not conflate the projects.

## WHAT EXISTS ALREADY IN SLERMES (actual, verified)

Located at `/c/Users/eman5/slermes/slermes/`:

```
src/
  main.c              — entry point
  agent/               — agent core (35 .c files)
    agent_loop.c       — core conversation loop (2175 LOC)
    llm_client.c       — LLM HTTP client + provider adapters
    provider*.c        — OpenAI, Anthropic, Google, Bedrock, custom, xai, deepseek, openrouter, gemini fallback
    context.c          — context window management
    context_engine.c   — context compaction/compression
    context_references.c
    vault.c            — memory vault
    tool_error.c       — error classification
    think_scrubber.c   — reasoning extraction
    redact.c           — PII redaction
    skill_bundles.c    — skill loading
  tools/               — tool implementations (45 .c files)
    exec_code.c        — code execution (Python via subprocess + bwrap sandbox)
    file.c, file_batch.c, file_merge.c, file_watch.c
    web.c, browser.c, web_search_registry.c
    mcp_tool.c         — MCP bridge
    delegate.c         — subagent delegation
    memory.c, todo.c, session_search.c, session_crud.c
    kanban.c           — kanban board
    computer_use.c     — computer use (mouse/keyboard/screenshots)
    image_gen.c, video_gen.c, video_analyze.c
    cronjob.c, send_message.c, tts.c, transcribe.c
    tirith.c           — security evaluator
    x_search.c, xai_http.c
  cli/                 — CLI entry points (TUI, fullscreen, commands)
  gateway/             — HTTP gateway + 20 platform adapters (telegram, discord, email, signal, etc.)
  cron/                — job scheduler
  mcp_serve.c          — MCP server
  sandbox_escape.c     — bubblewrap sandbox
include/               — 60+ headers (hermes_agent.h, hermes_json.h, hermes_http.h, etc.)
lib/                   — 75+ standalone C libraries (libjson, libhttp, libcrypto, libdb/sqlite3, libyaml, libregex, libuuid, libwebsocket, libmcp, etc.)
tests/                 — 340+ unit test files

Makefile               — cross-distro build (glibc, musl, macOS, Termux, Windows via MSVC vcvarsall)
  - 5-phase progressive build
  - Pattern rules for src/%.o, lib/%.o
  - Static linking target
  - Fuzz harness target
  - LDFLAGS: -lssl -lcrypto -ldl -lpthread -lz -lm
  - CFLAGS: -O2 -g -Wall -Wextra + many -Wno-* pragmas
```

## WHAT EXISTS IN ODYSSEUS (Python reference, to compare against)

Located at `/c/Users/eman5/slermes-odyssey/`:

Key files the other agent should READ before planning:

```
src/agent_loop.py        — streaming multi-round agent loop, fenced code block tool invocation
src/tool_execution.py    — tool dispatcher, subprocess runner, MCP bridge
src/tool_implementations.py — 50+ do_* functions
src/deep_research.py     — iterative Think→Search→Extract→Synthesize research engine
src/context_compactor.py — context window management for small models
src/memory.py            — dual-backend memory (JSON + ChromaDB/fastembed)
src/prompt_security.py   — untrusted content wrapping (40 LOC)
src/llm_core.py          — HTTP client, caching, dead-host cooldown, fallback
```

DO NOT re-read these — just understand the concepts.

## THE ACTUAL TASK

Create `slermes/slerm/` as a focused C extension module that fills the **specific gaps** between Odysseus' capabilities and slermes' current capabilities. This is NOT a "port 4 modules" exercise — it's a "what does the slermes agent actually need to be more capable" exercise.

## YOUR PHASES

### Phase 1: Code Review (read, don't write)

Read these files and note what each does well / poorly:
1. `src/agent/agent_loop.c` — how does the C agent loop work? Where does it handle tool calls? Where does it handle context overflow?
2. `src/agent/llm_client.c` — what providers? what's the API for making a call? can it do streaming?
3. `src/agent/context_engine.c` — does this already do compaction? If so, what can't it do?
4. `src/agent/vault.c` — how does memory work today? keyword-only? can it search semantically?
5. `src/tools/exec_code.c` — how does sandbox execution work?
6. `src/tools/mcp_tool.c` — how does MCP bridge work?
7. `src/tools/web.c` — how does web search work?
8. `Makefile` — how do I add a new `slerm/` subdirectory to the build?

For each, answer: does slermes already have this capability, partially have it, or not at all?

### Phase 2: Odysseus Comparison (read, don't write)

After the code review, compare ONLY the capabilities Odysseus has that slermes demonstrably lacks:

| Odysseus Feature | slermes Current State | Gap? | Gap Severity |
|-----------------|----------------------|------|--------------|
| Iterative deep research | None? | ? | High/Low |
| Context compaction for small models | context_engine.c exists | ? | Review needed |
| Untrusted content wrapping | None? | ? | High if missing |
| Semantic vector memory | vault.c exists | ? | Fine if keyword |
| LLM response caching | llm_core.c | ? | Review needed |
| Dead-host cooldown | llm_core.c | ? | Review needed |

You must VERIFY each cell by reading the actual C code. Do not assume.

### Phase 3: slerm/ Planning (write the plan, then the code)

Based on the verified gaps, write a precise plan. The plan should:
- List each gap clearly
- For each gap, decide: does it belong in `slerm/` or somewhere else in the existing codebase?
- If it belongs in `slerm/`, specify the exact C API (function signatures, structs)
- If it's already covered by existing code, say so and move on

**slerm/ position in the repo:**
```
slermes/slerm/
├── include/slerm_*.h    ← public headers
├── src/*.c              ← implementation
└── tests/slerm_test_*.c ← unit tests
```

`slerm/` is a linked library. Not an executable. Not a web server. It extends the existing agent via the established patterns in `src/agent/` and `src/tools/`.

## CONSTRAINTS

1. **C only.** No Python, no shell logic, no JS.
2. **Match existing slermes style exactly.** Same naming conventions, same error handling, same memory patterns (calloc/free), same JSON library (`lib/libjson/`), same Makefile patterns.
3. **No rewriting core.** `src/agent/agent_loop.c`, `src/agent/llm_client.c`, etc. are read-only reference.
4. **No web UI, no email, no calendar, no Docker.** Not relevant.
5. **Windows-safe.** Use `#ifndef _WIN32` / `#ifdef _WIN32` guards where needed. The Windows agent (OWL) compiles with MSVC Build Tools 2022.
6. **Wiki updates.** After each module, update `slermes-odyssey/.hermes/mind-palace/wiki/synthesis/slerm-odysseus-analysis.md` and `slermes-odyssey/.hermes/log.md`. Do NOT touch `slermes/slermes/.hermes/` — that's the Linux agent's mind palace.
7. **No prestige formatting.** Normal technical output only.
8. **No exaggeration.** If something already exists, say so. The Linux agent already maintains a bat···[truncated]