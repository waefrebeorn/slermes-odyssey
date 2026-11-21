# Odysseus — Prestige

## Purpose
Self-hosted AI workspace — local-first, privacy-first. Full-stack Python web app inspired by ChatGPT/Claude UI but running on your own hardware.

## Key Decision
Built by pewdiepie (Felix Kjellberg) as "Slermes Odyssey" — a Forest Gump moment where a gaming YouTuber stumbled into building a serious AI workspace. Combines opencode agent loop, llmfit hardware-aware model serving, and Tongyi DeepResearch into a single FastAPI + vanilla JS app.

## Project Identity
- **Name:** Odysseus (repo: slermes-odyssey)
- **Origin:** https://github.com/waefrebeorn/slermes-odyssey (fork of pewdiepie's Odysseus)
- **Pewdiepie's original:** https://github.com/pewdiepie-archdaemon/odysseus
- **Pewdiepie did NOT make:** bytropix or slermes — those are waefrebeorn's projects
- **Framework:** FastAPI (Python 3.11+) + vanilla JS frontend
- **License:** MIT + Apache-2.0 components (see ACKNOWLEDGMENTS.md)
- **Version:** 1.0

## Stack
```
Python 3.11+ | FastAPI | SQLite | ChromaDB | httpx | vanilla JS | Starlette
```

## Feature Areas
| Area | Description | Status |
|------|-------------|--------|
| Chat | Multi-model chat (local + API) | ✅ Working |
| Agent | opencode-based agent loop with tools | ✅ Working |
| Cookbook | Hardware-aware model download/serve | 🟡 Needs cross-platform testing |
| Deep Research | Multi-step research synthesis | ✅ Working |
| Compare | Blind model comparison | ✅ Working |
| Documents | Multi-tab editor with AI assist | ✅ Working |
| Memory/Skills | ChromaDB vector + keyword retrieval | ✅ Working |
| Email | IMAP/SMTP with AI triage | ✅ Working |
| Notes/Tasks | Quick notes, todos, scheduled tasks | ✅ Working |
| Calendar | CalDAV sync | ✅ Working |
| Mobile | PWA, responsive | ✅ Working |

## Priority Queue
| Priority | Item | Status |
|----------|------|--------|
| P0 | Squash bugs (CSS, edge cases) | 🔴 Active |
| P0 | Fresh install smoke tests (Linux/macOS/Windows/Docker/WSL) | ⬜ Needed |
| P0 | Integration audit — what actually works? | ⬜ Needed |
| P1 | Cookbook reliability across platforms | ⬜ Needed |
| P1 | Agent prompt/context bloat for small models | ⬜ Needed |
| P1 | Skill/tool prompt-injection audit | ⬜ Needed |
| P2 | CSS cleanup (Calypso's island) | ⬜ Needed |
| P2 | Dead code pass | ⬜ Needed |
| P3 | Accessibility pass | ⬜ Needed |

## Relationship to Slermes Ecosystem
```
slermes-odyssey (this)      ← fork of pewdiepie's Odysseus
slermes                     ← waefrebeorn's C Hermes Agent (headless)
bytropix                    ← waefrebeorn's Qwen3.6 inference engine (C)

Goal: Extract useful Odysseus features → slermes/slerm/ for bytropix math projects
```
