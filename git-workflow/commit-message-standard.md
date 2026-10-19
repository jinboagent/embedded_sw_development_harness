# Commit-Message Standard

Every commit message is a structured document, because agents and tools
read history to answer "why is this line here."

## Format (enforced by `githooks/commit-msg`)

```
type(scope): summary

## What Changed
- {{concrete bullets; file-level where useful}}

## Why (Decision/Rationale)
- {{the decision this commit encodes and what it closes/trades off}}

## How (Process)
- {{approach taken; alternatives rejected and why, when non-obvious}}

## Verification
- {{exact commands + counts; "python tests/... -> 42/42 OK"}}
```

- Types: `feat fix docs style refactor perf test build ci chore revert`.
- Scope: module or area (`firmware`, `host`, `charts`, `release`, …).
- The hook checks the subject regex, the four section headers, and that
  What Changed has at least one content line. Merge/Revert/fixup subjects
  are exempt.
- **Verification must be reproducible** — a reader with the parent commit
  should be able to run your commands and see your counts.
- **`fix` commits reference the bug ID** (`fix(scope): … (B1)`) from
  `bug_check/README.md` — the ID links the commit to its index row and
  fix report.

## Install the hook (once per clone)

```bat
git config core.hooksPath git-workflow/githooks
```

(Genuine exceptions bypass with `git commit --no-verify`.)

## Why enforced by hook and not convention

Audit result from the source project: 46 of 47 free-form commits drifted
off-format within weeks. Conventions decay; a 39-line shell hook does not.
