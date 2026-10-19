<!-- Template: fix report — copy to bug_check/bug_fix_report/fix-report-<date>.md.
     One per bug batch; links back to the index row in bug_check/README.md and
     names the verifying commits + counts. Edit points: everything. -->

# Fix Report — {{eval / bug IDs}}

| Field | Value |
|-------|-------|
| Date | {{YYYY-MM-DD}} |
| Covers | {{eval name / bug IDs B1–B3}} |
| Index row | [`bug_check/README.md`](../README.md) {{row}} |
| Fix commits | {{sha}} ({{agent}}) — {{one line each}} |

## What was wrong

- {{B1 — symptom → root cause}}

## What changed

- {{B1 — the fix, file-level}}

## Verification

- {{exact commands + counts, e.g. `{{test cmd}} → N/N`; hardware runs point
  at `harness/02-knowledge/evidence-…/`}}

## Residuals / deferred

- {{anything intentionally left open, with its accepted reason}}
