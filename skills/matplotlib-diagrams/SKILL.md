---
name: matplotlib-diagrams
description: Generate publication-quality block/flow diagrams as regenerable Python scripts (matplotlib), producing PNG+SVG with verified alignment, translatable label variants. Use when a document, README, article, or deck needs architecture/data-flow diagrams that must stay reproducible from source, ship in English (or multiple languages), and never contain hand-drawn misalignments.
---

# Matplotlib Diagrams (generator-as-source)

Diagrams are code: one Python file per diagram, deterministic renders,
width/alignment assertions instead of eyeballing, and label variants
(original language + English) for public assets.

## When to use

- A README/article/deck needs system-architecture or data-flow figures.
- The same diagram must exist in two languages (internal + public EN).
- Figures should regenerate after wording changes, not be re-drawn.

## The generator recipe (matplotlib)

- **Canvas:** 16×9 inches at DPI 150 → 2400×1350 px. One `fig`, one
  `ax` with `ax.set_xlim(0,100)`, `ax.set_ylim(0,100)`, aspect off,
  axis off. Work in a 0–100 coordinate space.
- **Boxes:** `FancyBboxPatch` with `boxstyle="round,pad=0.6"`, pastel
  fills, 1.5-pt edges; a helper `box(x, y, w, h, title, lines, style)`
  keeps styling uniform.
- **Arrows:** `FancyArrowPatch` with `arrowstyle="-|>"`,
  `mutation_scale=16`; dashed purple for simulation/feedback loops,
  solid for real data flow — and keep the legend honest about it.
- **Text:** helper functions only — never raw `ax.text` calls scattered
  around; every label goes through the same font-size/color constants.
- **Fonts:** DejaVu Sans is safe for English everywhere; for CJK
  originals use the platform CJK font (e.g. Microsoft YaHei) and keep
  the English variant on DejaVu.
- **Output:** save both PNG and SVG (same base name); print the written
  paths.

## Verification (do not skip)

1. **Box-width assertions:** every text line placed inside a box must
   satisfy `len(line) * char_width <= box_width` — assert it in the
   script; a ValueError at generation time beats a clipped label.
2. **Visual check after first render or any geometry change:** render,
   look at the PNG, fix overlaps (title vs subtitle, arrow-through-text,
   label clipped by box edge are the classic four).
3. **Variant parity:** if labels were translated, re-check — English
   strings are ~1.3× shorter than CJK, so boxes sized for one language
   overflow or underflow in the other.

## Conventions that paid off

- One file per diagram named `build_<name>[_en].py`; renders land next
  to the sources with matching names.
- Public-facing images are the EN variants, always — internal language
  stays internal.
- After any render, copy the exact figure files to the consuming
  location (article `img/`, deck `render/`) — never reference across
  trees.
- If the repo has chart governance, register image diagrams as
  presence-only rows so their existence is checked too.
