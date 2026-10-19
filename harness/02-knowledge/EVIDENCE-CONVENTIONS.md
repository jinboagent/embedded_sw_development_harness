# Evidence conventions

**The rule: nothing is "verified" without a transcript in this folder.**
Every verification claim in a report, commit message, or status file
references a file here. If there is no transcript, the claim is "expected",
not "verified".

## Folder naming

| Kind | Pattern | Example |
|------|---------|---------|
| Feature work | `evidence-<feature-slug>-<YYYY-MM-DD>/` | `evidence-parser-2026-01-15/` |
| Regression-gate runs | `evidence-<gate-name>/<YYYYMMDD_HHMMSS>/` | `evidence-hw-runs/20260115_143000/` |

## Inside a folder

- Numbered transcripts in execution order: `01-<what-happened>.log`,
  `02-<what-happened>.txt`, …
- The number prefix encodes order; the suffix is honest about content —
  a rejection transcript is named for the rejection
  (`01-forbidden-upload-rejected.log`), not sanded down to look like a pass.
- Failed runs are kept. A transcript of a failure that led to a fix is
  exactly the WHY a future session needs.
- Transcripts are captured verbatim (tee'd by the test runner where
  possible); never edited after the fact. Corrections go in the process
  report, not the transcript.

## What a transcript must contain

1. The command line that produced it (or the runner's banner carrying it).
2. Raw output, including the failure modes.
3. A final count/assertion summary the report can quote.
