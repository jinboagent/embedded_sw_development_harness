# Chart Registry — embedded-agent-kit

> The kit's own registry (dogfooding). Full policy + the blank template
> live in `tools/chart-registry.template.md`; the checker is
> `tools/check_charts.py`.

## Tracking table

| ID | Role | Status | Location | Style | Keep in sync with | Verified | sha8 | Rev |
|----|------|--------|----------|-------|-------------------|----------|------|-----|
| CH-readme-04 | master | current | `README.md:142-144` | unicode-box | the document-driven loop (doctrine section) | 2026-10-20 | 5c64bcbe | 1 |
| CH-readme-01 | master | current | `README.md:86-102` | unicode-box | the session loop description | 2026-10-20 | 585b50d9 | 1 |
| CH-readme-02 | master | current | `README.md:169-177` | unicode-box | the feature-branch policy flow | 2026-10-20 | 73d3b67c | 1 |
| CH-readme-03 | master | current | `README.md:207-215` | unicode-box | the context-tier table in agent-context/ignore-files.md | 2026-10-20 | ef055523 | 1 |
| CH-readme-zh-01 | Chinese variant of CH-readme-01 | current | `README_zh.md:62-78` | unicode-box | 会话循环（中文变体） | 2026-10-21 | 1ac44195 | 1 |
| CH-readme-zh-02 | Chinese variant of CH-readme-02 | current | `README_zh.md:115-123` | unicode-box | 功能生命周期（中文变体） | 2026-10-21 | 534651bc | 1 |
| CH-readme-zh-03 | Chinese variant of CH-readme-03 | current | `README_zh.md:148-156` | unicode-box | 上下文分层（中文变体） | 2026-10-21 | a225c916 | 1 |
| CH-readme-zh-04 | Chinese variant of CH-readme-04 | current | `README_zh.md:103-105` | unicode-box | 文档驱动循环（中文变体） | 2026-10-21 | 1fdac55d | 1 |
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


### CH-readme-01 · The session loop

```text
        you (the human)
        │  goal · review · the only deploy key
        ▼
┌───────────────┐  loads every session  ┌────────────────────────────────┐
│ coding agent  │◄─────────────────────►│ harness/00-global-context/     │
│               │                       │ rules · build · git workflow   │
│ edits code    │  loads on task match  ├────────────────────────────────┤
│ runs tests    │◄─────────────────────►│ harness/01-features/<stage>/   │
│ writes docs   │                       │ the spec + acceptance criteria │
└───────┬───────┘                       └────────────────────────────────┘
        │  every claim needs a transcript
        ▼
┌───────────────┐  tees transcripts      harness/02-knowledge/
│  test gate    │───────────────────────► evidence-<feature>-<date>/
└───────┬───────┘  + process report      <feature>-<date>.md  ("the why")
        ▼
 4-section commit ─► BRANCH manifest ─► squash-merge ─► FEATURES index
```

**Shows:** one working session end to end — the human sets the goal, the
agent reads constitution + spec, the gate produces transcripts, and the
commit/manifest/index chain records it.

**How to read it:** horizontal double arrows = what the agent loads;
downward flow = the path a change takes toward main; the right-hand
labels name the destination folders.

**Keep in sync with:** the tier descriptions in `harness/README.md`
(CH-harness-readme-01) and the evidence rule in
`harness/02-knowledge/EVIDENCE-CONVENTIONS.md`.

**Sites:** master `README.md` · status: current, verified 2026-10-20.

### CH-readme-02 · The life of a feature

```text
 proposal / spec            branch cut                 implementation
┌──────────────┐  approve  ┌───────────────┐  build    ┌───────────────┐
│ feature      │──────────►│ feature/<x>   │──────────►│ code + tests  │
│ template     │           │ + BRANCH.md   │           │ against ACs   │
└──────────────┘           └───────────────┘           └───────┬───────┘
                                                                │ gate green,
                                                                ▼ evidence filed
 FEATURES row   ◄────────  squash-merge  ◄──────────────────────┘
 (index updated)            (4-section message)
```

**Shows:** the branch ritual from spec to landed feature.

**How to read it:** left to right = time; the bottom return path is the
merge back to the baseline that updates the FEATURES index.

**Keep in sync with:** `git-workflow/feature-branch-policy.md` (the
merge ritual section) and the BRANCH/FEATURES templates.

**Sites:** master `README.md` · status: current, verified 2026-10-20.

### CH-readme-03 · Context economy

```text
   always loaded (small, stable)      on task match            never auto-loaded
┌─────────────────────────────┐ ┌──────────────────────┐ ┌──────────────────────┐
│ AGENTS.md / CLAUDE.md       │ │ 01-features/<stage>/ │ │ 02-knowledge/ (the   │
│ 00-global-context/          │ │ the one spec in play │ │ evidence library)    │
│ constitution: rules, build, │ │ acceptance criteria  │ │ 02-future/ (parking  │
│ workflow, privacy           │ │                      │ │ lot), archives,      │
│                             │ │ BRANCH manifest of   │ │ vendored code,       │
│                             │ │ the open branch      │ │ frozen proposals     │
└─────────────────────────────┘ └──────────────────────┘ └──────────────────────┘
```

**Shows:** what an agent loads, when, and what it never loads.

**How to read it:** three columns = three loading policies; the middle
column is per-task, the right column is opt-in by explicit read.

**Keep in sync with:** `agent-context/ignore-files.md` §2 (the tier
table) and the banner in `templates/02-future-README.template.md`.

**Sites:** master `README.md` · status: current, verified 2026-10-20.

### CH-readme-04 · The document-driven loop

```text
 intent ─► SPEC ─► agent ─► code ─► GATE ─► evidence ─► history
             ▲                                           │
             └────────── lessons become documents ◄──────┘
```

**Shows:** the doctrine in one line — documents drive generation, gates
demand proof, history accumulates, and lessons flow back as documents.

**How to read it:** top row = the forward path of one unit of work; the
bottom return line = self-refinement (a correction ends as a new
document, not a memory).

**Keep in sync with:** the doctrine bullets above it and the
process-report / tostudy templates it references.

**Sites:** master `README.md` · status: current, verified 2026-10-20.
