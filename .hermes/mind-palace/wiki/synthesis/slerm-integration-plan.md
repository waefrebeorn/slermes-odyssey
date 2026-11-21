# slerm/ Integration Plan — From Odysseus to Slermes

## Goal
Extract useful patterns from Odysseus (Python) and integrate them into `slermes/slerm/` (C) for bytropix math-project agent use.

## Target Architecture
```
slermes/
├── slerm/                   ← NEW: Math-project agent module
│   ├── agent_loop.c         ← Port of Odysseus agent loop
│   ├── agent_loop.h
│   ├── tool_dispatch.c      ← Tool execution engine
│   ├── tool_dispatch.h
│   ├── memory.c             ← Dual-backend memory (SQLite + vector)
│   ├── memory.h
│   ├── prompt_security.c    ← Untrusted context wrapping
│   ├── prompt_security.h
│   ├── deep_research.c      ← Iterative research synthesis
│   ├── deep_research.h
│   ├── context_compact.c    ← Context window management
│   ├── context_compact.h
│   ├── llm_client.c         ← LLM HTTP client (extend existing)
│   ├── llm_client.h
│   ├── tools/               ← Tool implementations
│   │   ├── bash_tool.c
│   │   ├── python_tool.c
│   │   ├── file_tool.c
│   │   ├── web_tool.c
│   │   ├── math_tool.c      ← NEW: Math-specific tools
│   │   └── ...
│   ├── tests/               ← Unit + integration tests
│   │   ├── test_agent_loop.c
│   │   ├── test_memory.c
│   │   ├── test_tools.c
│   │   └── test_research.c
│   └── Makefile             ← Build + test targets
├── src/                     ← Existing slermes C agent
│   ├── agent/               ← Existing agent loop
│   ├── cli/                 ← Existing CLI
│   ├── gateway/             ← Existing gateway
│   ├── tools/               ← Existing tool handlers
│   └── ...
└── Makefile                 ← Updated to build slerm/ as submodule
```

## Feature Porting Priority

### Phase 1: Core Agent Infrastructure (Weeks 1-2)
| Feature | Odysseus Source | slerm/ Target | Effort |
|---------|----------------|---------------|--------|
| Agent loop | `agent_loop.py` (2200 LOC) | `agent_loop.c` | High |
| Tool dispatch | `tool_execution.py` (870 LOC) | `tool_dispatch.c` | High |
| Subprocess runner | `tool_execution.py` | `tools/bash_tool.c` | Medium |
| Prompt security | `prompt_security.py` (40 LOC) | `prompt_security.c` | Low |
| Context compaction | `context_compactor.py` | `context_compact.c` | Medium |

### Phase 2: Memory + Research (Weeks 3-4)
| Feature | Odysseus Source | slerm/ Target | Effort |
|---------|----------------|---------------|--------|
| Dual memory backend | `memory.py` (370 LOC) | `memory.c` | High |
| Deep research | `deep_research.py` (830 LOC) | `deep_research.c` | High |
| LLM client extension | `llm_core.py` (1300 LOC) | `llm_client.c` | Medium |

### Phase 3: Math-Specific Tools (Weeks 5-6)
| Tool | Description | New? |
|------|-------------|------|
| `math_eval` | Evaluate mathematical expressions | ✅ New |
| `latex_render` | Render LaTeX to image | ✅ New |
| `gguf_inspect` | Inspect GGUF weights (pewdiepie's original interest) | ✅ New |
| `benchmark_run` | Run inference benchmarks | ✅ New |
| `paper_search` | Search arxiv for math papers | ✅ New |
| `proof_check` | Verify proof steps | ✅ New |

### Phase 4: Testing + Hardening (Weeks 7-8)
| Task | Description |
|------|-------------|
| Unit tests | Every module in slerm/ |
| Integration tests | Agent loop + tools + memory |
| Cross-platform | Linux + Windows |
| Security audit | Prompt injection, tool sandboxing |
| Performance | Benchmark vs Python Odysseus |

## Key Technical Challenges

### 1. Streaming SSE in C
Odysseus uses FastAPI's SSE for streaming agent output. In C:
- Use libhttp or mongoose for HTTP server
- Implement SSE protocol manually (`data: ...\n\n`)
- Or use WebSockets instead (simpler in C)

### 2. Vector Search in C
Odysseus uses ChromaDB (Python). In C:
- Option A: ONNX Runtime C API for fastembed embeddings + custom vector index
- Option B: SQLite FTS5 for keyword search (simpler, no embeddings)
- Option C: Use slermes's existing SQLite (hermes_state.py equivalent)

### 3. Subprocess Management in C
Odysseus uses asyncio subprocess. In C:
- `fork()` + `exec()` + `pipe()` on Linux
- `CreateProcess()` + pipes on Windows
- Need cross-platform abstraction

### 4. JSON Parsing in C
Odysseus uses Python's json module. In C:
- Use cJSON or jansson library
- Tool arguments are JSON → parse with cJSON
- LLM responses may be JSON → same

## What NOT to Port
| Feature | Reason |
|---------|--------|
| Web UI (FastAPI + JS) | slermes is headless; UI is separate concern |
| Email/Calendar | Not relevant for math projects |
| Cookbook | Hardware detection is Python-specific |
| Auth system | slermes has its own auth |
| 40+ route modules | Web routes are Python-specific |

## Success Criteria
1. slerm/ agent loop runs multi-round tool execution
2. Memory persists across sessions (SQLite + optional vector)
3. Deep research works with local LLM (bytropix)
4. Math tools (eval, latex, proof_check) work
5. All tests pass on Linux and Windows
6. No memory leaks (valgrind clean)
