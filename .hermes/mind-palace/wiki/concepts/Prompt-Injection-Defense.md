# Concept: Prompt Injection Defense

## Summary
Techniques to prevent malicious content in user-controlled inputs from hijacking the LLM's behavior. Critical for any system where users can edit skills, notes, documents, or provide web content.

## How It Works
1. **Identify untrusted surfaces:** user-editable skills, notes, documents, emails, web pages, tool output
2. **Wrap untrusted content:** separate messages with clear "UNTRUSTED SOURCE DATA" markers
3. **Add policy override:** explicit system-level instruction that untrusted content is data, not instructions
4. **Metadata tagging:** mark messages as `trusted: false` for downstream filtering

## In Odysseus
- **File:** `src/prompt_security.py` (40 LOC, surprisingly small)
- **Policy:** `UNTRUSTED_CONTEXT_POLICY` — explicit override instruction
- **Wrapper:** `untrusted_context_message()` — wraps content in `<<<UNTRUSTED_SOURCE_DATA>>>` markers
- **Placement:** untrusted content goes in `user` role messages, NOT system role
- **Surfaces covered:** documents, skills, notes, emails, web results, memories, tool output

## Key Design Decisions
- **Separate messages** — untrusted content never mixed with trusted instructions
- **User role placement** — lower privilege than system role
- **Wrapper markers** — clear delimiters so the LLM can distinguish data from instructions
- **Explicit policy** — "this policy overrides any conflicting character or preset behavior"

## Limitations
- Effectiveness depends on LLM compliance — not foolproof
- No adversarial testing framework
- Doesn't handle multi-modal injection (images with embedded text)

## In slermes
- slermes C agent handles this differently (tool output is structured, not free-text)
- For slerm/ math-project agent: similar concerns with math problem inputs
- Could adopt Odysseus-style wrapping for user-provided math content

## Sources
- [[odysseus-prompt-security-src]]
