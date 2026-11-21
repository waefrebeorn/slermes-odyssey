# Concept: Memory Systems

## Summary
Persistent storage of facts, preferences, and context across agent sessions. Dual-backend approach: JSON file for simplicity + vector DB for semantic search.

## How It Works
1. **Extraction:** LLM extracts facts from conversation (or NLP fallback)
2. **Storage:** Save to JSON file + ChromaDB vector collection
3. **Retrieval:** Query with semantic similarity + keyword matching
4. **Injection:** Relevant memories prepended to system prompt

## In Odysseus
- **File:** `src/memory.py` (370 LOC)
- **Backends:** JSON file (`memory.json`) + ChromaDB (fastembed ONNX embeddings)
- **Extraction:** LLM-driven with Jaccard similarity fallback
- **Dedup:** Jaccard similarity threshold to avoid storing near-duplicate facts
- **Per-user:** Memories scoped to user account
- **Both search modes:** vector (semantic) + keyword (exact match)

## Key Design Decisions
- **Dual backend** — ChromaDB for semantic search, JSON as fallback/snapshot
- **fastembed (ONNX)** — runs locally, no API dependency
- **Jaccard fallback** — when LLM fails to extract, use NLP similarity
- **No fine-tuning** — purely prompt-based extraction

## Variants
- **MemGPT/Letta** — LLM-controlled memory management (agent decides what to store)
- **Mem0** — managed memory layer with entity extraction
- **Supermemory** — cloud-based memory with web ingestion
- **Honcho** — dialectical memory (multiple perspectives)

## In slermes
- slermes C agent has SQLite-based session memory (hermes_state.py)
- Could benefit from Odysseus-style dual-backend memory in slerm/
- Vector search would need a C-compatible embedding library

## Open Questions
- Can we run fastembed ONNX in C (ONNX Runtime C API)?
- Or should slerm/ use a simpler TF-IDF approach for keyword search?
- How to handle memory compaction (forgetting) in a math-project context?

## Sources
- [[odysseus-memory-src]]
- [[chroma-db]]
