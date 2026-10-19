<!-- Template: bug tracking — copy to bug_check/README.md. The consolidated
     index: one row per evaluation, a stable short bug ID per finding, and a
     link to the fix report. Edit points: everything; the ID convention and
     the three lists (deferred / divergences / archive) are the reusable
     shape. The "reference the bug ID" rule pairs with the commit-message
     standard. -->

# Bug Tracking — {{PRODUCT}}

> Consolidated index. Latest state first: {{what is currently OPEN, or "none"}}.

## Status: {{current evaluation}} — {{OPEN / FIXED / partially}}

| Evaluation | Bugs | Outcome | Report |
|------------|:----:|---------|--------|
| {{eval + date}} | {{N}} | {{fixed / deferred / rejected, with commit sha}} | [`{{fix-report}}`](bug_fix_report/{{fix-report}}) |

## Bug-ID convention

Every finding gets a short, stable ID — `B1, B2, …` for bugs, or a batch
letter prefix per evaluation (`H1`, `N1`, `M1`) so IDs stay readable across
evaluations. A `fix` commit references the ID in its subject or body
(`fix(scope): … (B1)`). The ID is the link between the index row, the
commit, and the fix report — never reuse an ID.

## Deferred to v-next (documented, accepted)

| Item | Reason |
|------|--------|
| {{item}} | {{why deferred / accepted behavior}} |

## Known divergences (spec vs code, decisions pending)

1. {{divergence → current state → what decision is pending}}

## Archive

- `archive/{{…}}` — superseded evaluations and stale reports; keep, never delete.

## Fix reports

One file per bug batch under `bug_check/bug_fix_report/fix-report-<date>.md`
(template: `templates/fix-report.template.md`). Each links back to its index
row and names the verifying commits + test counts.
