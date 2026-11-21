# META PROMPT: slermes code review + slerm/ extension plan

## AGENT IDENTITY

You are reviewing and extending the slermes repository at `/c/Users/eman5/slermes/slermes/`. This is a C project — a production-grade C translation of the Hermes AI agent. Your job is to understand exactly what exists, what Odysseus (a Python reference) does differently, and what specifically needs to be added to a new `slerm/` subfolder inside this repo.

You are NOT rewriting the agent core. That already exists and works.
You ARE a focused extension layer on top of it.

## WHO MADE WHAT

- **pewdiepie** (Felix Kjellberg) made **Odysseus** (`github.com/pewdiepie-archdaemon/odysseus`). Fork: `waefrebeorn/slermes-odyssey`.
- **waefrebeorn** created **slermes** (this repo) and **bytropix** (a separate project, provided as coding reference only — do not focus on it).
- `slermes-odyssey` is the only overlap between these projects. Do not conflate them.

## WORKING DIRECTORIES

- `/c/Users/eman5/slermes/` — git worktree for this repo
- `/c/Users/eman5/slermes-odyssey/` — Odysseus Python fork (reference + wiki updates)

## ODYSSEUS (Python): what to understand

Key source files to skim for patterns:

```
src/deep_research.py     — Iterative Think→Search→Extract→Synthesize loop
src/context_compactor.py — Context window management (summarize old messages)
src/memory.py            — Dual backend: JSON + ChromaDB semantic search
src/prompt_security.py   — Untrusted content wrapping (40 LOC, very portable)
src/llm_core.py          — Response caching, dead-host cooldown, fallback chains
src/agent_loop.py        — Streaming multi-round tool execution (2200 LOC)
```

Read enough to understand the **concept** — do not port line-by-line.

## SLERMES (C): what exists RIGHT NOW

Located at `/c/Users/eman5/slermes/slermes/`:

```
src/
  agent/
    agent_loop.c         — core conversation loop (~2175 LOC). Handles: read message → send to LLM → tool_calls? → execute → loop
    llm_client.c         — HTTP LLM client
    provider_openai.c    — OpenAI adapter
    provider_anthropic.c — Anthropic adapter
    provider_google.c    — Google adapter
    provider_openrouter.c — OpenRouter adapter
    provider_deepseek.c  — DeepSeek adapter
    provider_custom.c    — Custom endpoint adapter
    provider_bedrock.c   — AWS Bedrock adapter
    provider_azure.c     — Azure adapter
    provider_xai.c       — xAI adapter
    context.c            — basic context management
    context_engine.c     — context compaction/compression
    vault.c              — memory vault (likely keyword-based)
    skill_bundles.c      — skill loading
    prompt_caching.c     — prompt caching support
    skill_commands.c     — skill command execution
    tool_error.c         — error classification
    think_scrubber.c     — strips reasoning tokens from output
  tools/
    exec_code.c          — Python execution via subprocess + bwrap sandbox
    web.c                — web search/fetch
    browser.c            — browser automation
    mcp_tool.c           — MCP bridge
    file.c, file_batch.c, file_merge.c, file_watch.c
    memory.c, todo.c
    delegate.c           — subagent delegation
    cronjob.c            — scheduled tasks (also in src/cron/)
    send_message.c, tts.c, transcribe.c
    tirith.c             — security evaluator
    kanban.c             — task board
  cron/                   — scheduler
  mcp_serve.c             — MCP server
  sandbox_escape.c        — bubblewrap sandbox

lib/ (75+ standalone libraries):
  libjson, libhttp, libcrypto, libdb (sqlite3), libyaml, libregex,
  libuuid, libwebsocket, libmcp, libprotobuf, libcsv, libhash, ...

include/ (60+ headers):
  hermes_agent.h, hermes_json.h, hermes_http.h, hermes_tool_*.h, ...

tests/ — 340+ unit test files

Makefile — cross-distro build:
  - 5-phase progressive build (phase1..phase5)
  - CFLAGS = -O2 -g -Wall -Wextra + many -Wno-* pragmas
  - Pattern rules: src/%.o, lib/%.o
  - LINK: -lssl -lcrypto -ldl -lpthread -lz -lm
  - Targets: all, phase1-5, tui, static, fuzz, clean
  - PHASE2_OBJ = DEPS_OBJ + src/main.o + AGENT_OBJ (35 files) + CLI_OBJ
  - AGENT_OBJ is a flat list of 45+ agent/*.o files
```

## THE ACTUAL TASK

Do a proper code review. Then decide what `slerm/` actually needs.

**Step 1 — Code Review (read, verify, document)**

Read these files and answer specific questions:

1. `src/agent/agent_loop.c` — Where does the tool-call loop live? Where does it check context overflow? Is there a compaction path? What's the max iterations mechanism?
2. `src/agent/context_engine.c` — What does it compact? How? Does it summarize or just truncate? What's the API?
3. `src/agent/vault.c` — How does memory storage work? What query API exists? Keyword search? Semantic?
4. `src/agent/llm_client.c` — Is there caching? Fallback chains? Streaming? Timeout handling?
5. `src/tools/exec_code.c` — How is the sandbox isolation done? bwrap paths?
6. `src/tools/web.c` — What web search backends exist?
7. `Makefile` — How are objects compiled? How would I add `slerm/src/*.c` and `slerm/tests/*.c` to the build? Where exactly do I insert lines?

For each, write: **what it does, what it does well, what it can't do, and whether the gap needs `slerm/`**.

**Step 2 — Odysseus Gap Analysis**

After the code review, produce a table:

| Odysseus Capability | Verified slermes State | Gap? | Does slerm/ need it? | Notes |
|--------------------|----------------------|------|----------------------|-------|
| Iterative deep research (Plan→Search→Extract→Synthesize) | ? | ? | ? | Read deep_research.py for algorithm |
| Context compaction for small models | ? (context_engine.c) | ? | ? | Check if it summarizes or truncates |
| Untrusted content wrapping | ? | ? | ? | Look for any prompt-injection defense |
| Semantic vector memory | ? (vault.c) | ? | ? | Check if FTS5 or plain SQLite |
| LLM response caching | ? (llm_core.c) | ? | ? | Check for cache key + TTL |
| Dead-host cooldown | ? (llm_core.c) | ? | ? | Check for failure tracking |

Fill in every cell by reading actual C code. No guessing.

**Step 3 — slerm/ Plan**

Write a concrete plan. The plan should:
- List ONLY the gaps that actually exist (after verification)
- For each gap: exact C API (headers + function signatures), file to create, test to write
- Include exact Makefile insertion points (line numbers or unique string anchors)
- Include a build verification command

**slerm/ placement:**
```
slermes/slerm/
├── include/
│   ├── slerm_research.h
│   ├── slerm_compact.h
│   ├── slerm_security.h
│   └── slerm_vector.h
├── src/
│   ├── research.c
│   ├── compact.c
│   ├── security.c
│   └── vector.c
└── tests/
    ├── slerm_test_research.c
    ├── slerm_test_compact.c
    ├── slerm_test_security.c
    └── slerm_test_vector.c
```

These are SUGGESTED names based on the Python source. If the code review reveals that something already exists under a different name in slermes (e.g., `context_engine.c` already handles compaction), **drop that module from slerm/ and document why**.

## CONSTRAINTS

1. **C only.** No Python, no shell logic, no JS.
2. **Match existing slermes style exactly.** Same conventions, same error patterns, same memory management (calloc/free), same JSON library (`lib/libjson/`), same Makefile patterns.
3. **No rewriting core.** `src/agent/agent_loop.c`, `src/agent/llm_client.c`, etc. are read-only reference.
4. **No web UI, no email, no calendar, no Docker.** Not relevant.
5. **Windows-safe.** Use `#ifndef _WIN32` / `#ifdef _WIN32` where needed. MSVC Build Tools 2022 is available.
6. **Wiki updates.** Update `slermes-odyssey/.hermes/mind-palace/wiki/synthesis/slerm-odysseus-analysis.md` and `slermes-odyssey/.hermes/log.md`. Do NOT touch `slermes/slermes/.hermes/` — that's the Linux agent's mind palace. Check with `ls .hermes/mind-palace/` first.
7. **No prestige formatting.** Normal technical output.
8. **No exaggeration.** If something already exists, say so and move on. Don't create work that isn't there.
9. **No bytropix focus.** bytropix is just an example of waefrebeorn's code. It's not the target.

## WORTHY OPPORTUNITIES (likely gaps, verify first)

These are the most likely candidates for `slerm/` based on what Odysseus has and slermes might be missing. VERIFY EACH ONE before including in the plan:

1. **Deep research engine** — slermes has web search but no iterative multi-step research synthesis. If absent, `slerm_research.c`.
2. **Untrusted content wrapper** — a 40-LOC security function. If absent, `slerm_security.c`.
3. **Context compaction** — slermes has `context_engine.c` but may only truncate, not summarize. If it only truncates, the gap is "LLM-driven summarization of old messages" → `slerm_compact.c`. If it already summarizes, drop it.
4. **Vector memory search** — `vault.c` may be keyword-only. If so, a bag-of-words cosine similarity layer on top is useful → `slerm_vector.c`.

## WHAT NOT TO ADD TO slerm/

- Agent loop core — already exists in `src/agent/agent_loop.c`
- LLM client — already exists in `src/agent/llm_client.c`
- Tool execution — already exists in `src/tools/`
- Web search — already exists in `src/tools/web.c`
- Gateway, CLI, MCP, cron, sandbox, auth — all exist
- Email, calendar, gallery, compare, mobile — not relevant
- ONNX runtime — too heavy for first pass

## MAKEFILE INTEGRATION (from actual Makefile)

The Makefile uses these patterns:
```makefile
# Source objects (flat list of src/%.o):
AGENT_OBJ = src/agent/agent_loop.o src/agent/llm_client.o ...

# Library objects (one .o per lib):
LIB_OBJ = lib/libjson/json.o lib/libhttp/http.o ...

# To add slerm/: add src/slerm/src/*.o to a new SLERM_OBJ list, then include in PHASE targets.
# Tests are compiled manually (check tests/Makefile or bottom of main Makefile for test rules).
```

Find the actual test compilation section in the Makefile before proposing changes.

## DELIVERABLE FORMAT

After the code review, output:
1. **Verified state table** — what exists vs. what doesn't
2. **slerm/ plan** — only the verified gaps, with exact APIs and file paths
3. **Makefile diff** — exact lines to insert for build integration
4. **Test plan** — one test file per module, what each must verify
5. **Wiki update notes** — what to append to Odysseus analysis page

## START

```bash
cd /c/Users/eman5/slermes/slermes
ls .hermes/mind-palace/ 2>/dev/null || echo "no mind palace here"
```

Then begin the code review. Read the files. Document what you find. Then write the plan.
