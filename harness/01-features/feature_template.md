<!-- Template: feature spec — copy to harness/01-features/<stage>/feature_<name>.md
     BEFORE implementing a feature. The discipline: acceptance criteria and
     test cases are written before the code, and the spec is the contract the
     branch manifest points at. Edit points: all {{...}} slots. -->

# Feature: {{name}}

## Basic Info

| Field | Value |
|-------|-------|
| Feature ID | {{F<N>.<seq>}} |
| Stage | {{stage}} |
| Layer | {{layer from coding_rules §5}} |
| Depends on | {{feature IDs}} |
| Status | {{proposed \| spec-locked \| implemented \| verified}} |

## Functional Description

{{2–5 paragraphs. What it does, when it runs, who/what triggers it.}}

{{State machine, if any — register the diagram:}}

```
<!-- chart-id: CH-{{feature-slug}}-01 rev1 -->
{{ASCII STATE MACHINE / FLOW}}
```

## I/O Definitions

### Error codes

| Code | Name | Meaning |
|------|------|---------|
| {{-N}} | {{...}}_ERR_{{...}} | {{...}} |

### Public API

```c
{{int {{module}}_{{verb}}({{args}});}}
```

| Function | Params | Returns | Notes |
|----------|--------|---------|-------|
| {{...}} | {{...}} | {{0 ok / -N}} | {{...}} |

## Acceptance Criteria

1. {{Observable, quantifiable rule.}}
2. {{...}}
3. {{...}}
4. The feature's tests pass host-side; {{hardware claims verified on the
   device with transcripts under harness/02-knowledge/evidence-{{slug}}/.}}

## Exception Scenarios (minimum coverage)

- NULL/invalid argument → {{code}}
- {{resource busy}} → {{code}}
- {{buffer full / overflow}} → {{behavior}}
- {{timeout}} → {{behavior}}
- {{init not done}} → {{code}}

## Non-Functional Constraints

| Constraint | Value |
|-----------|-------|
| {{latency/memory/size}} | {{...}} |

## Test Cases

| TC | Action | Expected |
|----|--------|----------|
| TC-1 | {{...}} | {{...}} |
| TC-2 | {{...}} | {{...}} |

## Build & Test Commands

```bat
{{how to build/run just this feature's tests}}
```
