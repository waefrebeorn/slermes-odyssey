# Odysseus — Testing

## Purpose
Test protocol for Odysseus.

## Key Decision
No comprehensive test suite exists. Testing is primarily manual/smoke-test. This is a major gap.

## Run Tests
```bash
# No formal test suite exists
# Manual testing via:
python -m uvicorn app:app --host 127.0.0.1 --port 7000
# Then use the UI to test each feature

# Docker smoke test
docker compose up -d --build
# Check: http://localhost:7000
```

## What Needs Testing
| Area | What to Test | Priority |
|------|-------------|----------|
| Chat | Multi-model chat, streaming | P0 |
| Agent | Tool execution, multi-round, timeouts | P0 |
| Cookbook | Model download, serve, hardware detection | P1 |
| Deep Research | Research synthesis quality | P1 |
| Email | IMAP/SMTP, AI triage | P1 |
| Memory | ChromaDB + JSON dual backend | P1 |
| Auth | Login, tokens, 2FA | P2 |
| Mobile | PWA, responsive layout | P2 |
| Windows | Native Python + Docker | P0 |

## 4-Phase DA Loop
For every claimed feature:
1. **Claim** — "Feature X works"
2. **Verify** — Actually run it, check output
3. **Risk** — What could break? What's untested?
4. **Mitigate** — What to do if it breaks

## Known Test Gaps
- No unit tests for agent loop
- No integration tests for LLM providers
- No cross-platform smoke tests
- No CSS/visual regression tests
- No prompt injection test suite
