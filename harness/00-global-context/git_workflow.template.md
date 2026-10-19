<!-- Template: git workflow — tier 0. Genericized from the source project's
     policy; points at the kit's git-workflow/ docs. Edit points: baseline
     name, stage vocabulary, report-storage table. -->

# Git Workflow

## Branching

One feature per branch. Branch naming: `feature/<short-name>` (or
`<stage>-<short-name>` with stages). Full policy — baseline registry,
branch manifest, merge ritual, agent pre-flight:
`git-workflow/feature-branch-policy.md`.

## Commit messages (enforced)

Format `type(scope): summary` + four sections (`## What Changed`,
`## Why`, `## How`, `## Verification`). The `commit-msg` hook rejects
anything less; enable once per clone:

```bat
git config core.hooksPath git-workflow/githooks
```

Details: `git-workflow/commit-message-standard.md`.

## Review checklist (per pull/merge)

- Correctness: {{error paths covered? boundary conditions? new rule in the
  matching spec?}}
- Architecture: {{layering respected? cross-module only via interfaces?}}
- Naming: {{matches the conventions table in coding_rules?}}
- Testing: {{new behavior covered by a test that fails without the change?}}
- Documentation: {{spec/status/registry updated where the change is visible?}}

## Process reports (mandatory per significant session)

Every significant session leaves a report in `harness/02-knowledge/`:

| Session type | Report goes to | Evidence goes to |
|--------------|----------------|------------------|
| feature work | `<feature>-<date>.{{AGENT_TAG}}.md` | `evidence-<feature>-<date>/` |
| hardware runs | (existing report) | `evidence-{{gate}}/<timestamp>/` |
| pure docs | (no report needed) | — |

Report shape: `harness/02-knowledge/process-report.template.md`.

## The learning journal (`docs/tostudy.md`)

After each significant session, append what deserved deeper study: topic
numbered sequentially, with Why / Context / What to learn / Resources /
Mini Project. Lessons learned the hard way get their own numbered entries
too — the journal is the project's institutional memory for its humans.

## Methodology reflection ritual

Periodically (monthly, or at stage boundaries), write a short reflection:
what in this process paid off, what created friction, what to change.
File it in `harness/02-knowledge/` as `methodology-<date>.{{AGENT_TAG}}.md`.
The process is itself under version control — iterate it deliberately.
