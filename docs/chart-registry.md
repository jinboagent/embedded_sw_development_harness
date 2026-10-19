# Chart Registry — embedded-agent-kit

> The kit's own registry (dogfooding). Full policy + the blank template
> live in `tools/chart-registry.template.md`; the checker is
> `tools/check_charts.py`.

## Tracking table

| ID | Role | Status | Location | Style | Keep in sync with | Verified | sha8 | Rev |
|----|------|--------|----------|-------|-------------------|----------|------|-----|
| CH-harness-readme-01 | master | current | `harness/README.md:5-8` | tree | the harness folder layout | 2026-10-20 | 96b06f4e | 1 |

## Catalog

### CH-harness-readme-01 · Harness three-tier tree

```text
harness/
├── 00-global-context/   the constitution — always in agent context
├── 01-features/         per-feature specs + acceptance criteria
└── 02-knowledge/        process reports + run-transcript evidence
```

**Shows:** the three tiers of project truth and their one-line jobs.

**How to read it:** plain folder tree; the annotation after each `——`
says when the tier is loaded/read.

**Keep in sync with:** the actual `harness/` folder structure — adding a
tier (e.g. `02-future/`) means updating this tree.

**Sites:** master `harness/README.md` · status: current, verified 2026-10-20.
