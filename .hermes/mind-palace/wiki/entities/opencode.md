# Entity: opencode (Agent Framework)

## Summary
Open-source AI coding agent by Anomaly. Provides the agent-loop and tool-execution patterns adapted for Odysseus.

## Key Facts
| Property | Value |
|----------|-------|
| Name | opencode |
| Author | Anomaly (anomalyco) |
| Original repo | opencode-ai/opencode (archived Sep 2025) |
| Current repo | anomalyco/opencode |
| License | MIT |
| Language | TypeScript (original), patterns adapted to Python in Odysseus |

## What Odysseus Adopted
- Agent loop pattern (multi-round tool execution)
- Tool invocation convention (fenced code blocks)
- Stream-based execution model

## What Odysseus Added
- Web UI (FastAPI + vanilla JS)
- Additional tools (email, calendar, tasks, skills, memory)
- ChromaDB-backed memory system
- Deep research engine
- Cookbook (hardware-aware model serving)
- Prompt injection hardening

## Sources
- [[opencode-repo]]
- [[odysseus-acknowledgments]]
