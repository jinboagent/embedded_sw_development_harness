# embedded-agent-kit

**A process kit for building software with AI agents — extracted from a
real hardware project where LLM agents developed, tested, and shipped
ESP32 firmware over months of sessions.**

You are about to build something with coding agents. They are fast,
tireless, and stateless — they forget rules between sessions, assert
"verified" without proof, and leave git history nobody can audit six
weeks later. This kit is the operating manual that fixes that: a set of
files that turn agent behavior from chat-history-dependent into
constitution-dependent.

Copy the folders in, fill the placeholders, install one hook — and your
agents inherit the same discipline that carried a six-stage firmware
project to 500+ passing checks with every claim backed by a transcript.

## The problem, in one paragraph

AI coding agents don't lack intelligence; they lack *institutions*. A
human team accumulates rules in onboarding docs, review habits, and
tribal memory. An agent team has none of that — every session starts
from whatever the agent reads. Without structure you get: rules that
live in chat scrolls (gone next session), "works on my machine"
verification claims, commits like `fix stuff`, diagrams that silently
drift from the code they explain, and a main branch nobody dares to
touch. Every element of this kit exists because one of those failures
happened for real in the source project, and the fix proved itself over
months.

## The philosophy — seven principles

1. **Agents need a constitution, not chat history.** The rules live in
   small, stable files the agent loads every session
   (`agent-context/` + `harness/00-global-context/`). If a rule matters,
   it's in a file. If it's in a file, it loads.
2. **Evidence or it didn't happen.** "Verified" means a transcript under
   `harness/02-knowledge/` — nothing else counts. Commit messages quote
   commands and counts; transcripts prove them.
3. **One feature, one branch, one manifest.** Every branch carries a
   `BRANCH` manifest (scope, acceptance criteria, changelog); every
   squash-merge lands one row in the `FEATURES` index. History stays an
   audit trail, not a stream.
4. **Commits are structured documents.** `type(scope): summary` plus
   What / Why / How / Verification — enforced by a git hook, because
   convention alone decayed within weeks in the source project.
5. **Diagrams rot silently — so register them.** Every ASCII diagram has
   an ID, a hash, and a checker that fails the build on drift.
6. **Status is a pointer, not a wiki.** `status/LATEST.md` points at the
   newest snapshot; snapshots are append-only.
7. **Context is a budget.** Agents load the constitution (small, always)
   plus the one spec they're touching — not the archives, not the
   evidence, not the parking lot.

## How a working session flows

<!-- chart-id: CH-readme-01 rev1 -->
```
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

The loop is the product: rules in, evidence out, history that explains
itself. The agent never "remembers" a rule — it re-reads one; never
"believes" a fix — it re-runs one.

## The life of a feature

<!-- chart-id: CH-readme-02 rev1 -->
```
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

Nothing lands on main except through this path — and the manifest +
index mean that a year later you can answer "what was promised, what
was proven, and where is the transcript" for any line of code.

## Context economy — what the agent actually loads

<!-- chart-id: CH-readme-03 rev1 -->
```
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

This is why the harness is split into tiers: an agent editing feature X
needs the constitution plus spec X — not the whole history. The history
lives one hop away, keyed by feature and date, for the session that
asks "why is it like this?".

*(Yes — the three diagrams above are registered in this repo's own
chart registry, `docs/chart-registry.md`, and
`python tools/check_charts.py` verifies them. The kit eats its own
cooking.)*

## Getting started (per new project)

1. Copy this kit's folders into the project root (skip what you don't need).
2. Fill every `{{PLACEHOLDER}}` — each template lists its **Edit points** at the top.
3. Author `AGENTS.*.md` + `CLAUDE.md` from the skeletons; keep the
   "stay in sync" header.
4. Install the commit hook once per clone:
   `git config core.hooksPath git-workflow/githooks`
5. Seed `docs/chart-registry.*.md` from `tools/chart-registry.template.md`,
   then `python tools/check_charts.py --scan` to adopt existing diagrams.
6. Write the first `status/status-<date>-<slug>.md` + `status/LATEST.md`.
7. Author the files the entry points promise: root `README.md`,
   `docs/quickstart.md`, `docs/tostudy.md` (all in `templates/`).
8. Register your first baseline in `FEATURES.*.md`; cut feature branches
   per `git-workflow/feature-branch-policy.md`.
9. Seed `bug_check/README.md` if you track bugs by ID.
10. Adapt `templates/unit_test_template.md` (Unity C) if the project is
    C firmware; otherwise replace with your stack's equivalent.
11. Set ignore files per `agent-context/ignore-files.md` (and copy
    `templates/.gitignore.template.md` → `.gitignore`).
12. Before anything goes public, run the privacy audit block in
    `harness/00-global-context/publishing_privacy.template.md`.

## What's in the box

| Folder | What it gives you |
|---|---|
| `agent-context/` | AGENTS/CLAUDE entry-point skeletons, agent-memory template, ignore-file/context-tier strategy |
| `git-workflow/` | Feature-branch policy, BRANCH/FEATURES manifest templates, commit standard + enforcing hook |
| `harness/` | The three-tier tree: constitution, feature specs, process reports + evidence rules |
| `templates/` | Fill-in files: feature spec, Unity C tests, status snapshots, tests README, quickstart, learning journal, root README, `.gitignore`, bug tracking + fix reports |
| `tools/` | Chart-registry checker + registry template (mechanical diagram governance) |

## Why it is the way it is

| Decision | Why |
|---|---|
| Markdown files, not a tool/framework | Agents read files natively; zero install, zero lock-in, diff-able in git |
| Hook-enforced commits, not convention | The source project audited 47 free-form commits: 46 had drifted. Conventions decay; a 39-line shell hook doesn't |
| Templates with `{{placeholders}}`, not scripts | You keep authorship; the kit shapes judgment, it doesn't automate it away |
| Append-only status & evidence | History you can edit is history you can't trust; corrections are new entries, not rewrites |
| Evidence rule ("transcript or it didn't happen") | Agents state confidence fluently; transcripts are the only currency that doesn't inflate |
| Chart registry + checker | Diagram drift is invisible until it misleads; a hash makes it a build failure |
| Small constitution, tiered loading | Context is the scarce resource; the design spends it deliberately |

## Provenance

Extracted from an ESP32-S3 BLE bridge firmware project where multiple
LLM agents (ZCode, Claude, DeepSeek, Qwen) built six stages of features
under this workflow — every merge gated by hardware tests, every claim
carrying a transcript. The kit was then independently reviewed by
another agent, whose additions (bug tracking, quickstart, learning
journal, and five defect fixes) were folded in after re-review. Treat it
as field-tested process, not theory.
