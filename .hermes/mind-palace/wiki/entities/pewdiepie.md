# Entity: pewdiepie (Odysseus Author)

## Summary
Felix Kjellberg (pewdiepie), gaming YouTuber with 111M+ subscribers, built Odysseus as an open-source AI workspace. Self-described as "I don't know what I'm doing hlep."

## Key Facts
| Property | Value |
|----------|-------|
| Name | Felix Kjellberg |
| YouTube | pewdiepie (111M+ subscribers) |
| GitHub | @pewdiepie-archdaemon |
| Original repo | github.com/pewdiepie-archdaemon/odysseus |
| Style | Casual, self-deprecating, built for personal use |

## The "Forest Gump Moment"
pewdiepie (Felix) stumbled into serious AI engineering:
1. Started with GGUF model internals (educational dissection)
2. Wrote Odysseus — the first autonomous self-hostable AI, open source, no trojan
3. Built into a full Python AI workspace wrapping opencode/llmfit/DeepResearch

Despite the humor ("I don't know what I'm doing"), the code is functional and the architectural decisions are sound.

## Relationship to waefrebeorn's Ecosystem

```
pewdiepie (felix) made: Odysseus.
waefrebeorn created independently:
├── slermes         — C translation of Hermes Agent (headless)
├── bytropix        — Qwen3.6 inference engine (C + CUDA)
└── slermes-odyssey — fork of pewdiepie's Odysseus (this repo)
```

**Key distinction:** pewdiepie made ONLY Odysseus. slermes and bytropix are **your** projects. slermes-odyssey is your fork of his Odysseus.

## Projects
| Project | Author | Description | Language |
|---------|--------|-------------|----------|
| [[Odysseus]] | pewdiepie | Self-hosted AI workspace | Python |
| [[slermes]] | waefrebeorn | C Hermes Agent translation | C |
| [[bytropix]] | waefrebeorn | Qwen3.6 inference engine | C + CUDA |
| [[slermes-odyssey]] | waefrebeorn | Fork of Odysseus (this repo) | Python |

## Relationships
- Created: [[Odysseus]]
- Inspired by: [[opencode]], [[llmfit]], [[Tongyi-DeepResearch]]
- Collaborates with: waefrebeorn (who maintains/only contributes to the fork)
