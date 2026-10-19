<!-- Template: PROJECT_SUMMARY — long-form agent memory for agents that
     support a persistent memory/settings folder (e.g. .qwen/, .claude/).
     Edit points: all sections. This file is gitignored by design — it is
     per-clone, per-agent state, not project truth (that lives in harness/). -->

# {{PROJECT}} — Agent Memory

> Gitignored per-clone memory for the {{AGENT}} agent. The project's real
> state lives in `status/LATEST.md` + `harness/`; this file carries the
> machine-specific gotchas and in-flight context that don't belong in the
> repo.

## Hardware / toolchain gotchas

- {{Gotcha: port resets on close; the accepted workaround}}
- {{Gotcha: toolchain quirk + the proven fallback invocation}}
- {{Gotcha: timing/limits discovered the hard way}}

## Architecture notes (short form)

- {{Module map in one or two lines}}
- {{Where the hot path is and what it must never do}}

## Conventions I must follow

- Commit standard + hook (see `git-workflow/commit-message-standard.md`)
- Evidence rule: every "verified" claim references a transcript under `harness/02-knowledge/`
- Chart registry: bump rev/sha8 + run the checker after touching any diagram
- Author-tagged filenames for my docs; never rename committed docs

## Recent actions

- {{date — what was just done, branch, commit}}

## Current plan

- {{What is in flight; what is blocked and on whom}}
