---
name: chart-registry-governance
description: Adopt and maintain a registry for ASCII/box-drawing diagrams in a repository, with a hash checker that fails on drift. Use when a project's markdown or code contains ASCII architecture/state/data-flow charts that must stay in sync with reality, when diagrams have silently rotted before, or after drawing a new chart.
---

# Chart Registry Governance

Every ASCII diagram in a *maintained* file gets an ID, a marker, a
registry row (with a content hash), and a catalog entry. A checker turns
"this diagram no longer matches the code" from a silent rot into a
mechanical finding.

## When to use

- A repo has ≥3 ASCII charts that people actually read (README,
  specs, docstrings) and that describe things that change.
- You just drew or edited a chart and need to register/refresh it.

## The pieces (ship with embedded-agent-kit)

- `tools/check_charts.py` — the scanner/checker (stdlib-only Python).
- `tools/chart-registry.template.md` — the registry skeleton
  (Policy + tracking table + catalog).

## Adopting into a project

1. Copy the checker into the project (e.g. `tools/`), and the template
   to `docs/chart-registry.md`. Retarget the checker's config constants
   (registry path, frozen/exempt paths, status vocabulary — each is
   marked with `KIT` comments).
2. Adopt existing charts: `python tools/check_charts.py --scan` lists
   every detected block with its sha8.
3. For each maintained chart: insert a marker line within 4 lines above
   the chart —
   - markdown/html: `<!-- chart-id: CH-<file-slug>-<NN> rev1 -->`
   - Python docstring: `chart-id: CH-<file-slug>-<NN> rev1`
   - C comments: `// chart-id: CH-<file-slug>-<NN> rev1`
4. Add one tracking-table row per chart site:
   `| CH-… | master | current | file:lines | style | keep-in-sync-with | date | sha8 | 1 |`
   (column order matters — the checker reads status and sha8 by index).
5. Paste each chart into the catalog section with: what it Shows, How to
   read it, Keep in sync with (the code/spec it mirrors), Sites.
6. Freeze point-in-time paths (archives, templates, proposals, vendored
   code) in the exempt lists — they are cataloged but never maintained.

## After changing a chart

1. Update the chart itself.
2. Re-paste the snapshot into its catalog section.
3. Bump the row: rev+1, today's date, new sha8 (from `--scan`).
4. `python tools/check_charts.py` — must report **0 findings**.

## Findings legend

- `MODIFIED` — hash differs from the row (update snapshot + sha8).
- `ORPHAN` — chart-shaped block with no marker in a maintained file.
- `GONE` — marker exists, chart vanished.
- `STALE` / `NO-ROW` / `MISSING` — registry out of sync with reality.

## Tips from the field

- Keep example markers in *templates* non-matching (use
  `CH-{{slug}}-01`, not a real-looking ID) or they read as GONE.
- Register image/mermaid diagrams as presence-only rows (empty sha8) —
  the checker verifies the site exists, not a hash.
- Run the checker in CI or the pre-merge ritual; drift that isn't
  checked is drift that returns.
