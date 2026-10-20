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

## What you gain by using it

- **Sessions start smart.** A new agent (or a new human collaborator)
  opens the repo and the rules are already there — no onboarding chat,
  no rule drift between agents or across months. In the source project,
  four different agents followed the same constitution without ever
  sharing a chat log.
- **"Verified" means something.** Every claim points at a transcript;
  every commit's Verification section is reproducible. You stop
  re-litigating whether something works.
- **History answers questions.** What was promised, what landed, why,
  and where the proof is — answerable from the FEATURES index, the
  branch manifests, and the 4-section commits a year later, without
  archaeology.
- **Silent drift becomes a loud failure.** Diagrams that no longer
  match the code, commits that skip Why or Verification — mechanical
  checks catch both, so nobody has to remember to.
- **Cheaper, more focused agent sessions.** Tiered loading spends
  context on the one spec in play, not the whole archive — smaller
  prompts, fewer "forgot the rule again" loops.
- **No lock-in, no infrastructure.** Plain markdown, one shell hook,
  one Python script. Any agent that reads files works with it; leave
  anytime and keep every file.
- **A process that proved itself.** Not theory: this workflow carried
  six stages of firmware to 500+ passing checks with every claim
  transcript-backed, and survived an independent agent review.

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

## AI-specific, document-driven development

When the executor is an LLM, documents stop being *documentation* and
become the **programming interface**. This is the doctrine behind the
whole kit:

- **The bottleneck moved from generation to verification and context.**
  Agents write code in seconds and assert correctness fluently. Output
  is multiplicative — *model capability × context quality* — so a strong
  model with zero context produces zero useful code. The highest-leverage
  engineering act is no longer writing the code; it is writing the
  documents the code will be generated from. (Tencent's QQ Music team
  reached the same conclusion independently and built an entire
  "Harness Engineering" governance layer on it — see the comparison
  below.)
- **Documents are executable inputs.** An agent doesn't read a spec for
  orientation — it loads it, plans against it, and is graded by it. So
  documents must be machine-consumable: acceptance criteria as checkable
  rules, error codes as tables, state machines as registered diagrams,
  rules as small always-loaded files. That is why this kit's templates
  are shaped the way they are.
- **Gates sit where errors are cheapest to stop.** A wrong assumption
  costs one edit at spec time, a rewrite at code time, an incident after
  merge. So every phase below ends in a *machine-checked* gate (hook,
  checker, test gate, human deploy key) rather than a convention —
  conventions decay; gates don't.
- **Knowledge compounds only when it's written down.** A lesson that
  lives in chat context dies with the session. Filed as a document — a
  process report, a learning-journal entry — it becomes an asset the
  next session re-reads. Every correction loop should end in a document.

<!-- chart-id: CH-readme-04 rev1 -->
```
 intent ─► SPEC ─► agent ─► code ─► GATE ─► evidence ─► history
             ▲                                           │
             └────────── lessons become documents ◄──────┘
```

The loop above is the whole idea: intent becomes a document, the
document drives generation, the gate demands proof, the proof becomes
history — and the lessons flow back as new documents.

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

Phase by phase, each step converts one document into the next — the
agent never works from memory or vibes:

| Phase | The document that drives it | The gate that ends it | Enforced by |
|---|---|---|---|
| 1. Idea → proposal | feature proposal: idea, rationale, **numbered decisions with the owner**, risks | decisions converge ("locked with the product owner") | human review |
| 2. Proposal → spec | feature spec from the template: acceptance criteria, error codes, test cases **written before the code** | spec locked; branch cut + manifest instantiated | agent pre-flight checklist |
| 3. Spec → code | the spec's ACs + the constitution (coding rules, layering, error ranges) | unit + contract tests green | test suite |
| 4. Code → proof | the manifest's verification plan | regression/hardware gate green, transcripts filed | gate runner + evidence rule |
| 5. Proof → history | the 4-section commit message | structure validated | `commit-msg` hook |
| 6. History → memory | process report + learning-journal entry | report exists and points at evidence | session ritual |

The gates are deliberately cheap to run and impossible to argue with:
phase 1 and 2 cost a review, phase 3 and 4 cost a command, phase 5
costs nothing (the hook fires anyway), phase 6 costs five minutes. The
expensive failure modes — wrong premise, unproven claim, unauditable
merge, lost lesson — are each intercepted at the phase where stopping
them is cheapest.

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

## How to use this repo

### Take it (one minute)

Copy the folders into a new project — nothing installs, nothing runs in
the background:

```bash
cp -r embedded-agent-kit/{agent-context,git-workflow,harness,templates,tools} /path/to/project/
```

or clone the kit and delete `.git`. Skim `templates/` first and drop
what your project won't use (Unity C tests don't apply to a Python
service; `bug_check/` is optional).

### One-time setup (per project)

1. Fill every `{{PLACEHOLDER}}` — each template lists its **Edit points** at the top.
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

### The day-to-day rhythm

- **Starting a session** — read `status/LATEST.md` (where things stand),
  the open `BRANCH` manifest (what's promised), and the one spec you're
  touching. Agents get the first two tiers automatically; humans get
  them in two files.
- **While working** — the constitution's rules apply (coding rules,
  layering, error ranges); if you change a registered diagram, bump the
  registry row and keep `check_charts.py` clean; run the tests before
  claiming anything.
- **Ending a session** — file the process report + evidence under
  `harness/02-knowledge/`, write a status snapshot and move the
  `LATEST.md` pointer, commit through the hook.
- **Merging a feature** — gate green → squash-merge with the 4-section
  message → add the FEATURES row → close the manifest's merge record.

### Where to look when

| You want to… | Go to |
|---|---|
| Start a new feature | `harness/01-features/feature_template.md` → cut a branch → `git-workflow/BRANCH.template.md` |
| Know why something exists | `harness/02-knowledge/` process reports (one per session, with transcripts) |
| Prove something works | `harness/02-knowledge/EVIDENCE-CONVENTIONS.md`, then the gate command |
| Change a diagram | marker + registry row + `python tools/check_charts.py` |
| Fix a bug | `templates/bug-tracking.template.md` index; `fix(scope): … (B1)` commit |
| Go public | the audit block in `harness/00-global-context/publishing_privacy.template.md` |
| Onboard a new agent | `agent-context/AGENTS.template.md` + `CLAUDE.template.md` |

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
journal, and five defect fixes) were folded in after re-review. The
document-driven doctrine section crystallized against Tencent QQ
Music's Harness Engineering write-up (2026) — an independent arrival
at the same principles, at platform scale. Treat it as field-tested
process, not theory.
