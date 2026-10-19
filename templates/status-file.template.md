<!-- Template: status snapshot — copy to status/status-<YYYY-MM-DD>-<slug>.md,
     then update status/LATEST.md to point at it. Snapshots are append-only:
     write a new file, never edit an old one. -->

# Status — {{YYYY-MM-DD}}: {{one-line state}}

| Field | Value |
|-------|-------|
| Date | {{YYYY-MM-DD}} |
| Branch | {{branch}} |
| Previous | `{{previous status file}}` |

## At a glance

- {{The 4–7 things that matter right now: milestones, open risks, where
  work is paused. Bold the numbers a returning reader scans for.}}

## Verification

- {{Exact counts per suite, with evidence pointers:
  "`{{suite}}` {{N}}/{{N}} — `harness/02-knowledge/evidence-.../`"}}

## Next

- {{Ordered next steps; each names its owner (human/agent) and any
  decision it is blocked on.}}
