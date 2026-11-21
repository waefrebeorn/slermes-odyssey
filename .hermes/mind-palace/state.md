# Odysseus — State

## Purpose
LIVE status dashboard. Update every session.

## Key Decision
This is a fork of pewdiepie's original. The original is at `github.com/pewdiepie-archdaemon/odysseus`. This fork is `waefrebeorn/slermes-odyssey`.

## Verification Tags
| Tag | Meaning |
|-----|---------|
| ✅ Verified | Confirmed working |
| 🟡 Partial | Works with known issues |
| ❓ Unchecked | Believed correct, not tested |
| 🔴 Broken | Known failure |
| ⚠️ DA Flag | Insufficient evidence |

## Metrics Dashboard
| Metric | Value | Verified |
|--------|-------|----------|
| Agent loop | 2200 LOC, 20 tool rounds max | ✅ |
| Tool types | 50+ native tools | ✅ |
| LLM providers | 10+ (vLLM, llama.cpp, Ollama, OpenRouter, OpenAI, etc.) | ✅ |
| Memory backends | JSON file + ChromaDB (fastembed ONNX) | ✅ |
| Research | Iterative Think→Search→Extract→Synthesize | ✅ |
| Cookbook | Hardware-aware model download/serve (llmfit-based) | 🟡 |
| Email | IMAP/SMTP with AI triage | ✅ |
| Calendar | CalDAV sync | ✅ |
| Mobile | PWA, responsive | ✅ |
| Windows native | launch-windows.ps1 exists | 🟡 |
| Docker | Multi-platform compose files | ✅ |

## Active Issues (from ROADMAP)
1. **CSS is "Calypso's island"** — needs major cleanup
2. **Agent context bloat** — tool schemas + skills + memory eat context for small models
3. **Cookbook reliability** — needs cross-platform testing
4. **Prompt injection** — user-editable skills/notes/documents are untrusted but need audit
5. **Dead code** — old routes, stale feature flags, unused UI states
6. **Mobile gallery/editor** — incomplete
7. **Accessibility** — no keyboard nav, focus states, contrast audit

## What Works Well
- Agent loop is solid: fenced code block tool invocation, streaming, multi-round
- Prompt security: untrusted context wrapping is well-implemented
- LLM core: dead-host cooldown, response caching, fallback chains
- Memory: dual backend (JSON + ChromaDB), Jaccard similarity for dedup
- Deep research: iterative synthesis with quality gating

## What Needs Work
- Windows native support (only Docker or WSL2)
- Cookbook cross-platform reliability
- CSS is a mess
- No comprehensive test suite for integrations
- Agent prompt bloat for small context windows
