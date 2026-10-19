<!-- Template: CLAUDE.md — context-optimized project overview for AI coding
     agents (Claude-family readers; same pattern works for any second agent).
     Edit points: {{PROJECT}}, the architecture narrative, status bullets,
     key-rules list, build block, key-files table. MUST stay in sync with
     AGENTS.<tag>.md — the rules sections are deliberately overlapping. -->

# {{PROJECT}}

> Context-optimized project overview for AI coding agents. Root `README.md`
> has build/usage details. New here? `docs/quickstart.md` is the 10-minute path.

## Architecture

{{2–4 sentences: the pipeline/control flow in arrows, where the intelligence
sits, what is deliberately thin.}}

| Tool | Role |
|------|------|
| {{tool A}} | {{one line}} |
| {{tool B}} | {{one line}} |
| {{tool C}} | {{one line}} |

## Current Status: v{{VERSION}} on {{main branch}}

- {{dated milestone, with verification numbers}}
- {{dated milestone, with verification numbers}}
- Verification inventory: {{unit counts}} · {{hardware counts}} · {{evidence path}}
- Next candidate work: {{...}}; see `status/LATEST.md`

## Key Rules

- **Reuse before building**: check platform built-ins and registries first
- **{{memory/allocation policy}}**
- **Error codes**: module-specific ranges in `{{interfaces}}`
- **{{safety/concurrency rule}}**
- **Host tools are self-contained by design** (copied helpers, no cross-imports); shared *conventions*, not shared code
- **Credentials** (`{{env file}}`, gitignored): {{resolution rules + known self-heals}}
- **Never commit directly to {{main}}**; one feature per branch with a root `BRANCH.{{AGENT_TAG}}.md` manifest; every squash-merge updates `FEATURES.{{AGENT_TAG}}.md`; every significant session leaves a process report + evidence in `harness/02-knowledge/`
- **ASCII charts are registered** (`docs/chart-registry.{{AGENT_TAG}}.md`): every diagram carries a `chart-id:` marker + a tracking row. Changed a chart → paste the new snapshot into its catalog section, bump rev/date/sha8, run `python tools/check_charts.py` (must stay clean)

## Build & Test

```bat
{{build}}                      :: main artifact
{{unit tests}}                 :: host unit suite ({{count}} tests)
{{hw gate}}                    :: hardware regression gate (device on {{PORT}})
{{agent entry command}}        :: start the interactive loop
```

{{The one machine-gotcha invocation, verbatim, with a sentence on when it's needed.}}

## Key Files

| Path | Purpose |
|------|---------|
| `{{src}}/` | {{modules}} |
| `harness/00-global-context/` | Product spec, coding rules, build env, git workflow, publishing/privacy + documentation rules |
| `harness/01-features/` | Per-feature specs ({{stages}}) |
| `harness/02-knowledge/` | Process reports + run-transcript evidence |
| `bug_check/README.md` | Consolidated bug tracking |
| `status/LATEST.md` | Latest status snapshot + pointer |
| `docs/quickstart.md` | 10-minute getting-started path |
