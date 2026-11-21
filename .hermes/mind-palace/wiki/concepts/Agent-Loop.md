# Concept: Agent Loop

## Summary
The core pattern for tool-using agents: stream LLM output, detect tool invocations, execute tools, feed results back, repeat until done or max rounds.

## How It Works
1. Send system prompt + conversation to LLM (streaming)
2. Accumulate response, watch for fenced code blocks with tool tags
3. On tool block: pause LLM stream, execute tool
4. Format tool result, append to conversation
5. Continue LLM stream or start new round
6. Repeat until LLM declares DONE/BLOCKED or max rounds reached

## In Odysseus
- **File:** `src/agent_loop.py` (2200 LOC)
- **Max rounds:** 20
- **Tool detection:** Fenced code blocks with language tag = tool name
- **Streaming:** SSE (Server-Sent Events) for real-time UI updates
- **Tool tags:** 50+ types (bash, python, web_search, read_file, write_file, etc.)
- **Timeout:** 60s default for bash/python, configurable per tool

## Key Design Decisions
- **Fenced code blocks** for tool invocation (not JSON function calls) — simpler for LLMs to emit
- **Preamble injection** tells LLM which tags are auto-executing vs. display-only
- **Round-based** with explicit DONE/BLOCKED/continue signals
- **Bias toward action** — rules tell LLM to just do it, not ask for clarification
- **Silent failure prevention** — failed tools must produce a follow-up, not go silent

## Variants
- **OpenAI function calls** — structured JSON, provider-native
- **MCP tools** — protocol-based tool servers
- **ReAct** — explicit Thought/Action/Observation cycles
- **Chain of Thought** → tool selection

## In slermes (bytropix)
- slermes C agent uses different pattern (CLI-based, not streaming)
- Could benefit from Odysseus-style streaming agent loop in slerm/
- Tool dispatch pattern is portable

## Open Questions
- Could the fenced-code-block pattern work in C (via regex parsing)?
- How to handle streaming SSE in a C HTTP server?
- Should slerm/ use JSON function calls instead (more structured)?

## Sources
- [[odysseus-agent-loop-src]]
- [[opencode-repo]]
