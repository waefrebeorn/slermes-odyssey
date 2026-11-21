# Concept: Deep Research

## Summary
Multi-step research synthesis: the LLM iteratively searches, extracts, and synthesizes information from multiple sources into a comprehensive report.

## How It Works (IterResearch pattern)
1. **Plan:** LLM breaks question into sub-questions and research plan
2. **Search:** Web search for each sub-question
3. **Extract:** LLM reads fetched pages, extracts relevant claims
4. **Synthesize:** Combine extracted information into draft answer
5. **Evaluate:** LLM assesses completeness against success criteria
6. **Iterate:** If gaps remain, search more; if complete, finalize

## In Odysseus
- **File:** `src/deep_research.py` (830 LOC)
- **Pattern:** Think→Search→Extract→Synthesize loop
- **Quality gating:** `is_low_quality()` filters low-information pages
- **Goal-based extraction:** `EXTRACTOR_PROMPT` focuses on question-relevant claims
- **Thinking strip:** `strip_thinking()` removes reasoning from final output
- **Adapted from:** Alibaba's Tongyi DeepResearch (Apache-2.0)

## Key Design Decisions
- **LLM-driven iteration** — LLM decides when to stop, not a fixed step count
- **Quality filtering** — skip low-yield pages early
- **Structured extraction** — claims with source attribution, not raw pastes
- **Success criteria** — LLM defines what "complete" looks like upfront

## Variants
- **Perplexity** — commercial deep research
- **OpenAI Deep Research** — GPT-based multi-step
- **Gemini Deep Research** — Google's version
- **STORM** — synthetic outline-driven research

## Potential for slerm/
- Math-project deep research: survey papers, find related work, compare approaches
- Could work with bytropix GGUF weights to run locally
- Natural fit for the "research" mode of a math-project agent

## Sources
- [[odysseus-deep-research-src]]
- [[tongyi-deep-research]]
