<!-- Template: quickstart — copy to docs/quickstart.md. The "10-minute path"
     that CLAUDE.md, AGENTS.*.md and the harness README all point at. Edit
     points: {{PRODUCT}}, the tool commands, the walk-through steps, the test
     commands. Keep it a path, not a manual — full details live in the root
     README; add one numbered section per feature you want the reader to see. -->

# Quickstart — {{PRODUCT}} in 10 minutes

{{One paragraph: what the reader will see with their own eyes by the end,
and the hardware/dependencies they need (device on {{PORT}}, toolchain,
libraries).}}

Full details live in the root `README.md`; this page is the path.

## 0. One-time setup (~3 min)

```bat
{{install dependencies}}
{{build command}}          :: main artifact
{{flash command}}          :: deploy to device ({{PORT}})
```

{{If there is a secrets/env file: where it goes (gitignored), the resolution
rule, and the known self-heals. If the project has a no-hardware/dry-run
mode, say so here.}}

## 1. {{first thing to see}} (~1 min)

```
{{command}}                :: {{what it does}}
```

{{1–2 sentences: what to type, what to watch for, and the one gotcha worth
knowing (e.g. a port/reset behavior).}}

## 2. {{next feature}} (~2 min)

```
{{command}}                :: {{what it does}}
```

{{What should happen, the expected numbers/output, and what the reader
should try to vary.}}

## Trust but verify (~1 min)

```bat
{{unit test command}}      :: {{count}} unit tests
{{hw gate command}}        :: the whole on-device battery, one command
```

Evidence transcripts for every claim above live in `harness/02-knowledge/`.

## Where to go next

- **Understand the design** — README *Architecture* onward, then
  `harness/01-features/{{stage}}/README.md`.
- **Change something** — read `CLAUDE.md` (rules + build recipes) and
  `harness/00-global-context/git_workflow.md` (branch → verify → process
  report → squash-merge; never commit to {{main}} directly).
- **Why was X built this way?** — `harness/02-knowledge/` holds a process
  report per session, with transcripts.
