<!-- Template: build environment record — tier 0. The section shape matters
     more than the content: installations → commands → issues+fixes →
     recommendations. Record the EXACT working invocation once you have it;
     that line saves every future agent session. Edit points: everything. -->

# Build Environment

## Installations

| Tool | Version | Path | Notes |
|------|---------|------|-------|
| {{toolchain}} | {{...}} | {{path}} | {{...}} |
| {{build system}} | {{...}} | {{path}} | {{...}} |
| {{host test toolchain}} | {{...}} | {{path}} | {{...}} |

## Command summary

```bat
{{build}}       :: main artifact
{{test}}        :: host unit suite
{{gate}}        :: full regression gate
{{flash/run}}   :: deploy to device ({{PORT}})
```

## Issues encountered (and their fixes)

| Symptom | Cause | Fix that worked |
|---------|-------|-----------------|
| {{error message}} | {{root cause}} | {{exact working command / workaround}} |

## Proven invocations (copy-paste, do not retype)

```
{{THE one weird-environment command that took an hour to find,
verbatim, with one sentence on when it is needed}}
```

## Recommendations

- {{Keep build dirs out of the repo; they're gitignored.}}
- {{Anything about reproducibility, pinned versions, CI parity.}}
