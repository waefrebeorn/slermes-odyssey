# Odysseus — Schema

## Purpose
Evolving schema for the Odysseus mind palace and LLM wiki. Defines conventions, rules, and workflows.

## Conventions

### File Naming
- All lowercase with hyphens
- Date-stamped plans: `plan-YYYY-MM-DD.md`
- Entity pages: `Entity-Name.md` (Pascal case with hyphens)
- Concept pages: `Concept-Name.md`
- Source pages: `source-short-name.md`

### Wikilinks
- `[[Page-Name]]` for internal links
- Link on first mention in each page
- Every entity/concept page needs ≥ 2 inbound links

### Verification Tags
| Tag | Meaning |
|-----|---------|
| ✅ Verified | Confirmed at runtime or against source |
| 🟡 Partial | Works with known caveats |
| ❓ Unchecked | Believed correct, not measured |
| 🔴 Broken | Known failure |
| ⚠️ DA Flag | Insufficient evidence for previous claim |

## Directory Structure
```
.hermes/
├── mind-palace/
│   ├── prestige.md          ← Full context resume
│   ├── entry.md             ← Build commands, architecture
│   ├── state.md             ← Live status, bugs, metrics
│   ├── plan.md              ← Phase roadmap
│   ├── testing.md           ← Test protocol
│   ├── project.md           ← Mission, done/pending
│   ├── index.md             ← Navigation hub
│   ├── wiki/
│   │   ├── Overview.md      ← Top-level wiki entry
│   │   ├── index.md         ← Content catalog
│   │   ├── entities/        ← Entity pages
│   │   ├── concepts/        ← Concept pages
│   │   ├── sources/         ← Source summaries
│   │   └── synthesis/       ← Cross-source analysis
│   ├── raw/                 ← Immutable sources
│   │   ├── papers/
│   │   └── articles/
│   ├── plans/               ← Date-stamped plans
│   ├── references/          ← Deep dives, HOWTOs
│   └── presentation/        ← Public-facing panel
├── schema.md                ← This file
├── log.md                   ← Append-only chronological record
└── vault/                   ← Achievements, history
```

## Workflows

### Ingest Workflow
1. Drop source into `raw/`
2. Read and extract key claims
3. Create/update source page in `wiki/sources/`
4. Update relevant entity/concept pages
5. Add `[[wikilinks]]` from new pages to related pages
6. Update `wiki/index.md`
7. Append to `log.md`

### Query Workflow
1. Read `wiki/index.md`
2. Read relevant wiki pages (not raw sources)
3. Synthesize answer with `[[citations]]`
4. File answer back into wiki if valuable

### Lint Workflow
1. Check for contradictions between pages
2. Check for stale claims
3. Check for orphan pages (no inbound links)
4. Check for missing pages (concepts mentioned but no page)
5. Check for missing links
6. Suggest new sources to fill gaps

## Rules
1. Raw files are never modified
2. Every session that changes something updates at least one walkway file
3. State files rot fastest — update aggressively
4. DA step is mandatory for every significant claim
5. Don't put task progress in agent memory — use walkway
6. If blocked, write a plan.md
