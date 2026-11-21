# Odysseus — Plan

## Purpose
Phase roadmap for integrating Odysseus features into the Slermes ecosystem.

## Key Decision
Odysseus is NOT directly usable for agentic math work — it's built backwards from GGUF dissection for educational purposes. The goal is to extract useful patterns and eventually merge them into `slermes/slerm/` for bytropix math projects.

## Phase Roadmap

### Phase 1: Analysis (Current)
- [x] Clone and read all key source files
- [ ] Create mind palace walkway
- [ ] Create LLM wiki (entity/concept pages)
- [ ] Write analysis: how pewdiepie made this, usefulness, shortcomings
- [ ] Identify extractable features for slerm/

### Phase 2: Feature Extraction Planning
- [ ] Map Odysseus features → slermes/slerm/ requirements
- [ ] Design slerm/ directory structure
- [ ] Identify which Python patterns translate to C
- [ ] Plan agent loop integration with slermes C agent

### Phase 3: slerm/ Implementation
- [ ] Create slerm/ folder in slermes
- [ ] Port agent loop patterns (tool execution, context management)
- [ ] Port memory system (dual backend)
- [ ] Port prompt security (untrusted context wrapping)
- [ ] Port deep research engine
- [ ] Integrate with bytropix inference

### Phase 4: Testing & Hardening
- [ ] Test slerm/ with bytropix math workloads
- [ ] Benchmark agent performance
- [ ] Security audit (prompt injection)
- [ ] Cross-platform testing (Linux + Windows)

## Dependency Graph
```
Phase 1 (Analysis) → Phase 2 (Planning) → Phase 3 (Implementation) → Phase 4 (Testing)
```

## Extractable Features (Candidates)
| Feature | Odysseus Implementation | slerm/ Value |
|---------|------------------------|--------------|
| Agent loop | `agent_loop.py` — streaming, multi-round, tool dispatch | Core agent pattern |
| Tool execution | `tool_execution.py` — subprocess, MCP, timeout, streaming | Tool infrastructure |
| Memory | `memory.py` — JSON + ChromaDB dual backend | Persistent context |
| Prompt security | `prompt_security.py` — untrusted context wrapping | Security hardening |
| Deep research | `deep_research.py` — iterative synthesis | Research capability |
| Context compaction | `context_compactor.py` | Long-context management |
| LLM core | `llm_core.py` — caching, fallback, dead-host | Provider abstraction |
| Cookbook | `services/hwfit/` — hardware-aware model selection | Model management |
