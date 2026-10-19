# embedded-agent-kit — a workflow & harness kit for AI-agent-built embedded projects

> Extracted from a hardware-proven ESP32-S3 project where multiple LLM
> agents developed firmware, host tools, and tests over months of sessions.
> This kit is the process layer that kept them coordinated — genericized
> for reuse. Copy it into any new project (embedded or not; the firmware
> flavor is optional).

## What's in the kit

| Folder | What it gives you |
|---|---|
| `agent-context/` | Entry-point files the coding agents read first: AGENTS/CLAUDE skeletons, a long-form agent-memory template, ignore-file strategy |
| `git-workflow/` | The strict branch ritual: feature-branch policy, branch manifest (BRANCH), feature index (FEATURES), and an enforced commit-message standard + hook |
| `harness/` | The three-tier spec/knowledge tree: global context (always loaded), per-feature specs, process reports + run evidence |
| `templates/` | Fill-in files: feature spec, Unity C unit tests, status snapshots, tests README, quickstart, learning journal (`tostudy`), project README, `.gitignore`, bug-tracking index + fix report |
| `tools/` | The ASCII-chart registry checker + registry template (mechanical diagram governance) |

## The philosophy in six lines

1. **Agents need a constitution, not chat history.** Rules live in files
   the agent loads every session (`agent-context/` + `harness/00-global-context/`),
   not in your memory of past conversations.
2. **Every claim carries evidence.** "Verified" means a transcript under
   `harness/02-knowledge/` — nothing else counts.
3. **One feature, one branch, one manifest.** The branch carries a
   `BRANCH.*.md` manifest; the squash-merge updates the `FEATURES.*.md`
   index; history stays readable.
4. **Commits are structured documents.** `type(scope): summary` plus four
   sections (What / Why / How / Verification), enforced by a hook.
5. **Diagrams rot silently — register them.** Every ASCII chart gets an
   ID, a hash, and a checker that fails on drift.
6. **Status is a pointer, not a wiki.** `status/LATEST.md` points at the
   newest snapshot; snapshots are append-only.

## Adoption checklist (per new project)

1. Copy this kit's folders into the project root (skip what you don't need).
2. Fill every `{{PLACEHOLDER}}` — each template lists its **Edit points** at the top.
3. Author `agent-context/AGENTS.*.md` + `CLAUDE.md` from the skeletons; keep the
   "keep in sync with each other" header.
4. Install the commit hook once per clone:
   `git config core.hooksPath git-workflow/githooks`
   (or move `commit-msg` into `.githooks/` at the root and use that path).
5. Seed `docs/chart-registry.*.md` from `tools/chart-registry.template.md`,
   then `python tools/check_charts.py --scan` to adopt existing diagrams.
6. Write the first `status/status-<date>-<slug>.md` + `status/LATEST.md`.
7. Author the files every entry-point file points at, from `templates/`:
   the root `README.md` (`README.template.md`), `docs/quickstart.md`
   (`quickstart.template.md`), and `docs/tostudy.md` (`tostudy.template.md`).
8. Register your first baseline in `FEATURES.*.md`; cut feature branches
   from it per `git-workflow/feature-branch-policy.md`.
9. Seed `bug_check/README.md` from `templates/bug-tracking.template.md` if
   you track bugs by ID (fix reports use `templates/fix-report.template.md`).
10. Adapt `templates/unit_test_template.md` (Unity C) if the project is
    C firmware; otherwise replace with your stack's equivalent.
11. Set ignore files per `agent-context/ignore-files.md` (and copy
    `templates/.gitignore.template.md` → `.gitignore`) so agents don't
    load archives/vendored code into context.
12. Before anything goes public, run the privacy audit block in
    `harness/00-global-context/publishing_privacy.template.md`.

## Conventions used across the templates

- `{{PROJECT}}` — project/product name; `{{AGENT_TAG}}` — your agent's
  file suffix (`zcode`, `claude`, `deepseek`, …); `{{MODULE}}` — a firmware
  module or subsystem name; `{{PORT}}`/`{{TOOLCHAIN}}` — machine specifics.
- Files marked **VERBATIM** are working code/docs copied from the source
  project with only an *Adaptation notes* banner added — edit the banner's
  listed points, not the whole file.
- Markdown authored by an agent carries an author-tagged filename
  (`*.{{AGENT_TAG}}.md`) and an author line — multi-agent repos stay
  attributable.
