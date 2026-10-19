<!-- Template: chart registry — copy to docs/chart-registry.{{AGENT_TAG}}.md.
     Seed it by running:  python tools/check_charts.py --scan
     then paste the discovered blocks below as catalog sections. -->

# Chart Registry — {{PROJECT}}

> Policy + tracking table + catalog for every ASCII/box-drawing diagram in
> the repo. The checker (`tools/check_charts.py`) turns chart drift from a
> silent rot into a build failure.

## Policy

- **ID scheme:** `CH-<file-slug>-<NN>`, one per chart *site* (composed
  from the location, so the ID is stable and self-describing).
- **Role:** `master` (the maintained copy) · `alias of CH-…` (a duplicate
  that must stay in sync) · `frozen copy of CH-…` (point-in-time).
- **Status:** `current` · `stale` (content behind reality) · `untracked`
  (not yet adopted) · `frozen-proposal` / `historical` / `template`
  (exempt from the checker) · `frozen`.
- **Update rule:** changed a chart → paste the new snapshot into its
  catalog section below, bump the row's Rev + Verified date + sha8 (from
  `--scan`), and run `python tools/check_charts.py` — it must report
  0 findings.
- **Exempt paths:** {{archives, templates, proposals, vendored code}} —
  point-in-time records, never maintained.
- **Marker forms:** `<!-- chart-id: CH-… rev1 -->` (markdown/html) ·
  `chart-id: CH-… rev1` (docstrings) · `// chart-id: CH-… rev1` (C).
- **Findings legend:** MODIFIED (hash mismatch) · ORPHAN (unmarked chart
  in a maintained file) · GONE (marker vanished) · STALE (status says
  stale — reminder) · NO-ROW / MISSING (registry out of sync).

## Tracking table

| ID | Role | Status | Location | Style | Keep in sync with | Verified | sha8 | Rev |
|----|------|--------|----------|-------|-------------------|----------|------|-----|
| CH-{{slug}}-01 | master | current | `{{file}}:{{lines}}` | {{unicode-box \| ascii \| tree}} | {{what code/spec it mirrors}} | {{date}} | {{sha8}} | 1 |
| CH-{{slug}}-02 | alias of CH-{{slug}}-01 | current | `{{file}}:{{lines}}` | unicode-box | {{…}} | {{date}} | {{sha8}} | 1 |

*(Presence-only rows — mermaid/image sites with no hashable text — leave
sha8 empty; the checker verifies the marker exists.)*

## Catalog

### CH-{{slug}}-01 · {{Title}}

```text
{{THE CHART — last-verified snapshot, pasted verbatim}}
```

**Shows:** {{2–3 plain sentences: what this diagram depicts.}}

**How to read it:** {{legend — what arrows mean, what boxes are, the
dashed/solid distinction if any.}}

**Keep in sync with:** {{the code/spec/feature that must change together
with this chart — "when you change X, update this chart."}}

**Sites:** master `{{file}}`{{ · alias `{{file}}`}} · status: current,
verified {{date}}.
