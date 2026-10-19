<!-- Template: documentation & language rules — tier 0. Edit points: the
     language choice, author-tag vocabulary, chart-registry paths. -->

# Documentation & Language Rules

## Language

- All repo markdown, docs and code comments are written in {{LANGUAGE —
  recommend English for anything that might ever be public}}. Chat with
  the user may stay in the working language; the repo does not.
- Any image or diagram aimed at readers outside the repo — article,
  presentation deck, release docs — must use {{LANGUAGE}} labels. Internal
  originals may stay in another language, but the public asset is always
  the translated variant.

## New files

- Agent-authored markdown is author-tagged: `*.{{AGENT_TAG}}.md` for this
  agent, `*.<other-agent>.md` for others, plus an author line under the
  title. Never rename or delete committed docs.

## ASCII chart governance

Every ASCII/Unicode box chart in tracked files is registered. Full policy
and the chart catalog live in `docs/chart-registry.{{AGENT_TAG}}.md`; the
mechanical checker is `tools/check_charts.py`.

- Each chart site carries a `chart-id: CH-<file-slug>-<NN>` marker next to
  the chart and one row in the registry's tracking table (location,
  master/alias role, status, sha8, rev, verified date).
- The registry catalog is the single reading copy: every distinct chart
  extracted, with a plain explanation, a how-to-read legend, and
  keep-in-sync instructions.
- Changed a chart → update the chart AND paste the new snapshot into its
  catalog section, bump the row's rev/date/sha8, then run:

  ```
  python tools/check_charts.py    # must report 0 findings
  ```

- New chart → draw it, add the marker, add a row + catalog section, re-run
  the checker. Renamed file → update the row's Location column, keep the ID.
- Frozen copies (archives, templates, point-in-time proposals) are
  cataloged, never maintained, exempt from the checker.
- Image/mermaid diagrams are registered as presence-only rows — the
  checker verifies the site still exists, not a hash.
