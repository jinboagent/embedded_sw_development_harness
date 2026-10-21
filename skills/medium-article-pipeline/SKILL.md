---
name: medium-article-pipeline
description: Turn a project into a Medium-import-ready article page - self-contained HTML using only import-safe elements (tables converted to bold-lead lists, figures with relative img paths), with the publish instructions embedded as a comment. Use when writing a technical article for Medium (or any importer-based platform) from a repo's README, docs, and figures.
---

# Medium Article Pipeline

Medium's importer pulls a hosted HTML page and converts it. Feed it a
page built for the importer, and formatting survives; feed it arbitrary
HTML, and tables degrade into mush.

## When to use

- Publishing a technical write-up on Medium from project material.
- The article must reuse figures that already exist (README images,
  deck renders, generated diagrams).

## The import-safe element set

Use ONLY: `h1/h2/h3`, `p`, `pre/code`, `blockquote`, `ul/ol/li`,
`figure/img/figcaption`, `em/strong/a`. In particular:

- **NO tables** — Medium degrades them to flat lists. Convert every
  table to a **bold-lead bullet list**: `<li><strong>Axis</strong> —
  value A; value B; value C.</li>` Reads fine on Medium, survives
  import.
- One `h1` only (the title); `h2` per section, `h3` for subsections.
- Inline `<style>` is preview-only (the importer ignores it) — keep it
  minimal so local preview ≈ imported result.

## Page structure that worked repeatedly

1. `<head>`: title, author, meta description; a **publish-instructions
   comment** (host the folder → medium.com/p/import → paste URL →
   canonical link comes back).
2. Opening italic summary paragraph → TL;DR blockquote.
3. Body sections; figures as
   `<figure><img src="img/….png" alt="…"><figcaption>…</figcaption></figure>`.
4. Closing "sources" paragraph with the links.

## Assembly rules

- **Self-contained folder**: `index.html` + `img/` beside it, relative
  paths only; commit them together (the imgs are part of the page).
- Figures: reuse the project's generated diagrams/renders — copy into
  `img/`, never cross-link. All-English labels for public figures.
- Write captions argumentatively, not descriptively: each caption should
  advance the article's point, not name the picture.

## Verification before publishing

- Script-check: every `<img>` resolves under `img/`; all tags balanced;
  `<table` count == 0; no non-target-language characters (for an
  English article: zero CJK); one h1; figure count == intended.
- Read the local render once at article width.
- Secrets/PII pass over the final text (names, paths, keys).

## Publish flow

1. Host the folder (GitHub Pages from the repo, or any static host).
2. Medium → story → "Import a story" → paste the public URL.
3. Review the draft (check figures arrived and lists read well), then
   publish; Medium links back to the hosted page as canonical.
