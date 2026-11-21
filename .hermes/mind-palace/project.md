# Odysseus — Project

## Purpose
Mission statement and big picture for Odysseus within the Slermes ecosystem.

## Key Decision
Odysseus is a Python AI workspace built by pewdiepie. It's NOT the agent — it's the UI wrapper around an agent. The real agent logic is in slermes (C). Odysseus provides patterns we can learn from and eventually port.

## Mission
Self-hosted AI workspace: local-first, privacy-first, no trojan. Combines chat, agent, research, email, calendar, notes, and model management in a single web UI.

## Origin Story
- Built by **pewdiepie** (Felix Kjellberg) — gaming YouTuber with 111M subscribers
- Originally at `github.com/pewdiepie-archdaemon/odysseus`
- Forked to `waefrebeorn/slermes-odyssey`
- pewdiepie's own words: "I don't know what I'm doing hlep"
- Despite the humor, the codebase is surprisingly competent — ~10K+ LOC of functional AI workspace

## Done
- [x] Full FastAPI web app with 40+ routes
- [x] Streaming agent loop with 50+ tool types
- [x] Multi-model LLM support (10+ providers)
- [x] Memory system (JSON + ChromaDB)
- [x] Deep research engine (iterative synthesis)
- [x] Cookbook (hardware-aware model download/serve)
- [x] Email (IMAP/SMTP with AI triage)
- [x] Calendar (CalDAV sync)
- [x] Notes/Tasks with scheduling
- [x] Prompt injection hardening
- [x] Context compaction
- [x] Mobile PWA support
- [x] Docker deployment

## Pending (from ROADMAP)
- [ ] Bug fixes (CSS, edge cases)
- [ ] Cross-platform smoke tests
- [ ] Integration audit
- [ ] Cookbook reliability
- [ ] Agent context bloat reduction
- [ ] Prompt injection audit
- [ ] CSS cleanup
- [ ] Dead code removal
- [ ] Accessibility pass

## Constraints
- Python 3.11+ required
- No Windows native GPU support (Docker can't use Metal on macOS)
- CSS is acknowledged as a mess
- No formal test suite
- Built for personal use, not production-hardened

## Relationship to Slermes
```
odysseus (Python, UI-focused, agent wrapper)
    ↓ patterns, not code
slermes (C, headless, agent core)
    ↓ integration target
slerm/ (future: math-project agent in slermes)
    ↓ uses
bytropix (C inference engine for Qwen3.6 math)
```
