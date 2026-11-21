# Odysseus — Entry

## Purpose
How to build, run, and understand Odysseus.

## Key Decision
Docker recommended for production. Native Python for development. Windows needs WSL2 or native Python + manual dependency management.

## Run Commands
```bash
# Docker (recommended)
cp .env.example .env
docker compose up -d --build
# → http://localhost:7000

# Native Linux/macOS
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python setup.py
python -m uvicorn app:app --host 127.0.0.1 --port 7000

# Apple Silicon (native for Metal GPU)
./start-macos.sh
# → http://127.0.0.1:7860

# Windows (native)
# See launch-windows.ps1
```

## Key Files
| Path | Purpose | LOC |
|------|---------|-----|
| `app.py` | FastAPI app, route registration, middleware | ~1000 |
| `src/agent_loop.py` | Streaming agent loop (tool execution) | ~2200 |
| `src/agent_tools.py` | Tool facade (re-exports) | ~130 |
| `src/tool_execution.py` | Tool dispatcher + subprocess runner | ~870 |
| `src/tool_implementations.py` | All `do_*` tool functions | ~4100 |
| `src/llm_core.py` | LLM HTTP client, streaming, caching | ~1300 |
| `src/memory.py` | MemoryManager (JSON file + ChromaDB) | ~370 |
| `src/prompt_security.py` | Prompt injection hardening | ~40 |
| `src/deep_research.py` | Iterative research engine | ~830 |
| `src/context_compactor.py` | Context window management | ~200 |
| `core/database.py` | SQLite ORM models | ~500 |
| `core/auth.py` | AuthManager (bcrypt) | ~200 |
| `routes/` | 40+ route modules | varies |
| `services/hwfit/` | Hardware detection + model fit scoring | varies |
| `services/research/` | Deep research service | varies |
| `static/` | Vanilla JS frontend | varies |

## Architecture
```
Browser → FastAPI (app.py)
              ├── routes/ (40+ route modules)
              ├── src/agent_loop.py → stream_llm() + tool execution
              │     ├── src/llm_core.py (httpx → LLM provider)
              │     ├── src/tool_execution.py (dispatcher)
              │     │     └── src/tool_implementations.py (do_* functions)
              │     ├── src/agent_tools.py (facade)
              │     ├── src/prompt_security.py (injection hardening)
              │     └── src/context_compactor.py (context management)
              ├── src/deep_research.py (iterative research)
              ├── src/memory.py (MemoryManager)
              ├── core/database.py (SQLite)
              ├── core/auth.py (AuthManager)
              ├── services/hwfit/ (hardware + model fit)
              └── services/research/ (research service)
```

## Dependencies
```
fastapi | uvicorn | httpx | sqlalchemy | bcrypt | python-dotenv
chromadb | fastembed (ONNX) | markitdown | pypdf | python-docx
```

## Environment Variables
| Variable | Purpose |
|----------|---------|
| `APP_BIND` | Bind address (default: 127.0.0.1) |
| `APP_PORT` | Port (default: 7000) |
| `AUTH_ENABLED` | Enable auth (default: true) |
| `DATABASE_URL` | SQLite/PostgreSQL URL |
| `ODYSSEUS_ADMIN_USER` | Admin username (default: admin) |
