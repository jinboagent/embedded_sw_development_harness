<!-- Template: root README — the human-facing overview with build/usage
     details. Edit points: everything. The agent-facing overview lives in
     CLAUDE.md / AGENTS.*.md; this file is for people, and both point back
     here ("root README.md has build/usage details"). Keep the quickstart
     pointer. -->

# {{PRODUCT}}

{{One paragraph: what it is, what it does, who uses it, and the closed loop
if there is one.}}

{{Optional hero image + caption.}}

## Status: v{{VERSION}} — {{stages}} complete

- {{2–4 bullets: what is implemented and verified, with the counts a
  returning reader scans for}}
- New here? `docs/quickstart.md` is the 10-minute path; per-feature specs
  live in `harness/01-features/`.

## Architecture

<!-- chart-id: CH-{{file-slug}}-01 rev1 -->
```
{{ASCII architecture diagram — register it: see docs/chart-registry.<agent>.md}}
```

{{A paragraph walking the data/control flow, and the reason the product
exists.}}

### Modules (`{{src}}/`, public APIs in `{{interfaces}}/`)

| Module | Component | Role |
|--------|-----------|------|
| {{module}} | {{component}} | {{one line}} |

### Layer model (dependency rules)

{{The layering diagram / rules from coding_rules §5 — or a one-line pointer
to `harness/00-global-context/coding_rules.<agent>.md`.}}

## Getting started

See `docs/quickstart.md` (10 minutes). One-time setup:

```bat
{{build}}                :: main artifact
{{flash}}                :: deploy to device ({{PORT}})
```

## Testing

| Layer | Command | What it covers |
|-------|---------|----------------|
| Host unit | `{{cmd}}` | {{...}} |
| Hardware | `{{cmd}}` | {{...}} |
| Regression gate | `{{cmd}}` | whole battery, one command |

See `tests/README.md` for the full inventory + conventions.

## Project layout

| Path | Purpose |
|------|---------|
| `{{src}}/` | {{implementation modules}} |
| `{{interfaces}}/` | Public APIs — the only cross-module surface |
| `host_app/`, `{{tools}}/` | Host-side tooling (self-contained scripts) |
| `tests/` | Host unit + hardware suites |
| `harness/` | Specs, global context, process reports + evidence |
| `status/LATEST.md` | Current state snapshot + pointer |

## Contributing / workflow

One feature per branch, 4-section commit messages, evidence-backed
verification — see `harness/00-global-context/git_workflow.md`.

## License & privacy

{{License}}. Before anything public, run the audit in
`harness/00-global-context/publishing_privacy.<agent>.md`.
