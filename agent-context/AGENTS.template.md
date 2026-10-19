<!-- Template: AGENTS.<agent>.md — the entry point file for coding agents.
     Edit points: {{PROJECT}}, {{AGENT}}, the layout table, build commands,
     the rules list, the current-state example. Keep the "keep in sync"
     header: if you also run a CLAUDE.md (or other agent files), they must
     carry the same rules. Target: <100 lines — agents load it every session. -->

# AGENTS.{{AGENT_TAG}}.md — {{PROJECT}}

> Author: {{AGENT}} · {{DATE}} · Workspace instructions for {{AGENT}} agents.
> Keep in sync with `CLAUDE.md` (same project, other agent readers). Full
> specs live in `harness/`; `docs/quickstart.md` is the 10-minute path.

## What {{PROJECT}} is

{{One paragraph: what the product is, the data/control flow in arrows
(e.g. input → processing → output), and the one-sentence reason it exists.
Name the closed loop if there is one.}}

## Layout

| Path | Purpose |
|------|---------|
| `{{src}}/` | {{implementation modules}} |
| `{{include-or-interfaces}}/` | Public APIs — the only cross-module surface |
| `{{app-entry}}/` | Init chain + main loop |
| `host_app/` + `{{tools}}` | Host-side tooling (self-contained scripts) |
| `tests/host/`, `tests/hw/` | Unit suites; hardware suites ({{PORT}}) |
| `harness/00-global-context/` | Product spec, coding rules, build env, git workflow, publishing/privacy + documentation rules |
| `harness/01-features/` | Per-feature specs ({{stage folders}}) |
| `harness/02-knowledge/` | Process reports + run-transcript evidence (the WHY behind commits) |
| `status/LATEST.md`, `BRANCH.{{AGENT_TAG}}.md`, `FEATURES.{{AGENT_TAG}}.md` | Current state, active branch manifest, merged-feature index |

## Build & test

```bat
{{build command}}       :: firmware / main artifact
{{unit test command}}   :: host unit suite
{{hw gate command}}     :: hardware regression gate (device on {{PORT}})
```

- {{Machine gotcha #1 — the exact working invocation, verbatim.}}
- {{Machine gotcha #2 — e.g. port behavior, toolchain quirk.}}

## Rules that matter for edits

- **Reuse before building**: check platform built-ins and the component registry first.
- **Memory**: {{allocation policy — e.g. static buffers for app logic}}.
- **Error codes**: module-specific ranges in `{{interfaces}}` ({{examples}}).
- **{{Safety-critical rule}}**: {{e.g. sandbox policy, concurrency rules}}.
- **ASCII charts are registered** (`docs/chart-registry.{{AGENT_TAG}}.md`): each
  diagram has a `chart-id:` marker + a tracking row (hash + rev). After
  changing a chart, update its catalog snapshot + row and run
  `python tools/check_charts.py` (must stay clean).
- **Credentials** (`{{env file}}`, gitignored — never commit): {{how they resolve}}.

## Git workflow (strict)

- **Never commit directly to {{main branch}}.** One feature per branch, cut from a
  registered baseline; every feature branch carries a root `BRANCH.{{AGENT_TAG}}.md`
  manifest; squash-merge, then update the root `FEATURES.{{AGENT_TAG}}.md` index and
  keep the branch. Policy: `git-workflow/feature-branch-policy.md`.
- Every significant session leaves a process report + evidence in
  `harness/02-knowledge/`.
- Commit message standard: `git-workflow/commit-message-standard.md` (hook-enforced).

## Doc conventions

- New markdown authored by an agent gets an author-tagged filename
  (`*.{{AGENT_TAG}}.md` for this agent) plus an author line. Never rename
  or delete committed docs.
- Before changing sensitive areas, read first: `harness/00-global-context/`,
  the matching spec under `harness/01-features/`, and `status/LATEST.md`.
- Repo docs and public-facing images are {{LANGUAGE}}-only, and nothing that
  ships may carry credentials or personal info. Details:
  `harness/00-global-context/publishing_privacy.template.md` +
  `harness/00-global-context/documentation_rules.template.md`.
