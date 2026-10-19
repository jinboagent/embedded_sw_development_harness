<!-- Template: process report — one per significant session, filed in
  harness/02-knowledge/ as <topic>-<YYYY-MM-DD>.{{AGENT_TAG}}.md.
  The five sections are the whole point; keep them in this order. -->

# {{Topic}} — {{YYYY-MM-DD}}

| Field | Value |
|-------|-------|
| Date | {{YYYY-MM-DD}} |
| Branch | {{branch}} |
| Feature | {{feature ID / "session"}} |
| Result | {{one line: landed / blocked / exploratory}} |
| Evidence | `harness/02-knowledge/evidence-{{slug}}-{{date}}/` |

## What was done

- {{Concrete bullets, commit shas where landed.}}

## Decisions made

- {{Decision → why → what it closes or trades off. Number them if more
  than three; future readers cite decision numbers.}}

## Problems encountered and resolutions

- {{Problem → root cause → fix. Unresolved problems say so explicitly
  and name the owner of the next step.}}

## Test results

- {{Exact commands and counts, e.g. `{{test cmd}} → {{N}}/{{N}} OK`.}}
- {{Hardware runs: point at the evidence dir; never claim without a
  transcript.}}

## Lessons learned

- {{What surprised us; what goes into docs/tostudy.md as a numbered
  lesson.}}
