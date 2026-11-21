# Entity: Odysseus

## Summary
Self-hosted AI workspace — full-stack Python web app combining chat, agent, research, email, calendar, notes, and model management. Built by pewdiepie as an open-source project.

## Key Facts
| Property | Value |
|----------|-------|
| Language | Python 3.11+ |
| Framework | FastAPI + vanilla JS |
| Author | pewdiepie (Felix Kjellberg) |
| Original repo | github.com/pewdiepie-archdaemon/odysseus |
| Fork | github.com/waefrebeorn/slermes-odyssey |
| License | MIT + Apache-2.0 components |
| Version | 1.0 |
| LOC (Python) | ~15,000+ across src/, core/, routes/, services/ |
| LOC (JS) | ~5,000+ in static/ |
| Agent loop LOC | ~2,200 (agent_loop.py) |
| Tool implementations LOC | ~4,100 (tool_implementations.py) |

## Component Breakdown
| Component | File(s) | Purpose |
|-----------|---------|---------|
| Web app | `app.py` | FastAPI routes, middleware, static files |
| Agent loop | `src/agent_loop.py` | Streaming multi-round tool execution |
| Tool dispatch | `src/tool_execution.py` | Subprocess runner, MCP bridge |
| Tool implementations | `src/tool_implementations.py` | 50+ do_* functions |
| LLM client | `src/llm_core.py` | httpx-based, caching, fallback |
| Memory | `src/memory.py` | JSON + ChromaDB dual backend |
| Research | `src/deep_research.py` | Iterative Think→Search→Extract→Synthesize |
| Context | `src/context_compactor.py` | Context window management |
| Security | `src/prompt_security.py` | Untrusted context wrapping |
| Auth | `core/auth.py` | bcrypt-based auth manager |
| Database | `core/database.py` | SQLite ORM |
| Routes | `routes/*.py` (40+ modules) | HTTP endpoints per feature |
| Hardware | `services/hwfit/` | GPU detection, model fit scoring |

## Relationships
- Adapted from: [[opencode]], [[llmfit]], [[Tongyi-DeepResearch]]
- Patterns for: [[slermes]] C agent
- Integration target: [[slerm]] math-project agent
- Uses: [[FastAPI]], [[ChromaDB]], [[httpx]]

## Sources
- [[odysseus-readme]]
- [[odysseus-roadmap]]
- [[odysseus-acknowledgments]]
- [[pewdiepie-archdaemon-repo]]
