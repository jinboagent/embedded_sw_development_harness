# The harness — three tiers of project truth

<!-- chart-id: CH-harness-readme-01 rev1 -->
```
harness/
├── 00-global-context/   the constitution — always in agent context
├── 01-features/         per-feature specs + acceptance criteria
└── 02-knowledge/        process reports + run-transcript evidence
```

## Read-before-edit rule

Before changing sensitive areas, read in this order:

1. `00-global-context/` — coding rules, build env, git workflow,
   publishing/privacy, documentation rules.
2. The matching spec under `01-features/{{stage}}/`.
3. `status/LATEST.md` + the active `BRANCH.*.md` for current state.

## What each tier is for

| Tier | Written when | Read when | Volatility |
|------|--------------|-----------|------------|
| 00-global-context | once, then rarely | every session (auto) | lowest |
| 01-features | before/during a feature | when touching that feature | per-feature |
| 02-knowledge | at session end | when asking "why" | append-only |

The separation exists for context economy: an agent editing feature X
needs the constitution (small, stable) + spec X (focused), not the whole
history. The history lives one hop away, keyed by feature and date.

## Naming conventions

- Specs: `feature_<name>.md` inside `stage<N>-{{theme}}/` folders.
- Process reports: `<topic>-<YYYY-MM-DD>.{{AGENT_TAG}}.md`.
- Evidence dirs: `evidence-<feature-slug>-<YYYY-MM-DD>/` with numbered
  transcripts (`01-<what>.log`, `02-<what>.txt`, …); hardware gate runs:
  `evidence-{{gate-name}}/<YYYYMMDD_HHMMSS>/`.
