<!-- Template: tests README — lives at tests/README.md. Three sections:
     inventory, why-each-layer-exists, conventions. The "why" section is
     the part most projects skip and most need: each test layer is
     justified by the real bug class it prevents. -->

# Tests — layers, gates, and conventions

## Layers

| Layer | Where | What it covers | Run it |
|-------|-------|----------------|-------|
| Host unit (C) | `tests/host/` | {{modules}} with stubbed dependencies | `{{cmd}}` |
| Host contract | `tests/host/` | {{wire/response contracts, fuzz}} | `{{cmd}}` |
| Host unit (script) | `tests/host/` | {{host-tool logic over simulators}} | `{{cmd}}` |
| Hardware | `tests/hw/` | {{on-device behavior via {{PORT}}}} | `{{cmd}}` |
| Soak | `tests/hw/` | {{long-window stability}} | `{{cmd}} --secs {{N}}` |
| Regression gate | `tests/hw/run_all.py` | whole battery, one command | `{{cmd}}` |

## Why each layer exists

- **Host unit** — {{bug class it catches: e.g. logic errors caught in
  seconds without hardware}}.
- **Contract tests** — {{e.g. every response validated against the wire
  format; caught the encoder bug that ... }}.
- **Hardware suites** — {{bug class: real radio/RTOS behavior that
  cannot be stubbed}}.
- **Soak** — {{bug class: fragmentation/windowing bugs invisible in
  short runs}}.
- **Gate** — one command a tired engineer will actually run before merge:
  asserts identity, sequences suites, checks {{reset/watchdog}} after
  each, tees transcripts.

## Conventions

- **Every verification claim references a transcript under
  `harness/02-knowledge/` — nothing is "verified" without one.**
- {{Response assertions are strict (full parse, not substring match) —
  substring matching let a stale-line bug through once.}}
- {{Responses matched to the command that issued them (correlation
  field), never "the first line that looks right".}}
- {{Deterministic seeds for anything random; flaky tests are fixed or
  marked, never ignored silently.}}
- {{Port-ownership rule: one program owns {{PORT}} at a time; suites
  document their settle/wait points.}}
- Status files are append-only; evidence transcripts are immutable.
