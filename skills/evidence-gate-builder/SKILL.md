---
name: evidence-gate-builder
description: Build a one-command regression gate for a device-under-test or any verifiable system - a meta-runner that asserts identity, sequences test suites as subprocesses, checks reset/watchdog health after each suite, tees transcripts as evidence, and exits nonzero on any failure. Use when a project has multiple test suites against real hardware or an external system and needs one trustworthy pre-merge command.
---

# Evidence Gate Builder

The pattern behind a gate that a tired engineer will actually run before
every merge. One command; it either proves the system or fails loudly
with transcripts.

## When to use

- Multiple test suites run against a device / service / board over a
  serial port, network, or other flaky boundary.
- Verification claims need transcripts behind them ("evidence rule").
- Unexpected reboots/resets of the device-under-test must fail the run.

## The meta-runner shape (single Python file, stdlib only)

1. **Own the resource for the whole run.** One program holds the port /
   connection at a time; the gate is that program for its duration.
2. **Assert identity first.** Probe a version/ID command and compare
   against an expected value — wrong firmware stops everything before a
   single suite runs.
3. **Run each suite as a subprocess** (not imports): isolation, own
   timeouts (e.g. 900 s), own exit codes. `SUITES = [(file, description), …]`.
4. **Settle between suites.** A short sleep (2–5 s) after each suite
   lets the device drain buffers and quiesce.
5. **Health-check after each suite.** Query the device's reset/watchdog
   reason; anything other than the expected power-on reason fails the
   run — a suite that rebooted the device silently is a failure even if
   its checks passed.
6. **Tee every transcript** to `<evidence-root>/<YYYYMMDD_HHMMSS>/<suite>.txt`
   so every claim in the summary has a file behind it.
7. **Print one summary table** (suite / counts / duration) and exit
   nonzero if anything failed. Zero surprises, zero interpretation.

## Conventions that made it hold up in the field

- Suites respond to `--port` / equivalent flags; the gate passes them.
- Per-suite expected check counts are printed by the suites themselves —
  the gate only collects, never re-counts.
- Flaky-by-nature checks are *informed SKIPs* inside suites, never
  silent passes.
- Long-window stability (soak) suites live beside the gate but run
  separately with explicit durations.
- Add `--only suite…` and `--quick` flags; a gate that can't be run
  partially gets skipped when people are in a hurry.

## Wiring into the workflow

- The branch policy names the gate as the merge condition.
- Commit-message Verification sections quote the gate summary +
  evidence directory.
- The evidence rule: nothing is "verified" without a transcript under
  the evidence root — the gate is what produces them.
