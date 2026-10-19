<!-- Template: coding rules — tier 0. The section skeleton is the reusable
     part; every table starts empty for your project to fill. Edit points:
     all tables, module names, ranges. -->

# Coding Rules

## 1. Error Codes

Common errors shared across all modules:

| Code | Name | Meaning |
|------|------|---------|
| 0 | OK | Success |
| -1 | ERR_NULL_PTR | NULL pointer argument |
| -2 | ERR_INVALID_ARG | Invalid argument value |
| -3 | ERR_NO_MEM | {{static buffer exhausted / allocation failed}} |
| -4 | ERR_BUSY | Resource busy |
| -5 | ERR_TIMEOUT | Operation timed out |
| -6 | ERR_NOT_INIT | Module not initialized |
| -7 | ERR_ALREADY_INIT | Module already initialized |

Module-specific ranges (each module owns a 100-wide band so parallel
authors never collide):

| Module | Range | Prefix |
|--------|-------|--------|
| {{module A}} | -100 to -199 | {{A}}_ERR_* |
| {{module B}} | -200 to -299 | {{B}}_ERR_* |
| {{module C}} | -300 to -399 | {{C}}_ERR_* |

## 2. Memory Rules

- **Application code**: {{policy — e.g. prefer static allocation; no malloc in business logic}}.
- **Libraries**: {{what's allowed where}}.
- **NULL check** every pointer at every module boundary → `ERR_NULL_PTR`.
- **Length check** every buffer write. Never write past the declared size.

## 3. Buffer Sizes

| Buffer | Size | Declared in |
|--------|------|-------------|
| {{...}} | {{...}} | `{{interfaces}}/{{...}}_if.h` |

## 4. Naming Conventions

| Kind | Convention | Example |
|------|-----------|---------|
| public function | {{prefix_verb_object}} | {{...}} |
| static helper | s_... | {{...}} |
| type | {{...}}_t | {{...}} |

## 5. Layering

```
Layer N:   {{top — user-facing commands/apps}}
Layer N-1: {{orchestration}}
Layer 2:   {{services}}
Layer 1:   {{drivers}}
Layer 0:   {{interfaces/*_if.h — shared types & contracts}}
```

Upper layers depend on lower layers only — no reverse dependencies; the
build system's `REQUIRES` lists are the visible dependency graph.
Cross-module data flows through interface types only.

## 6. Platform

- Toolchain/runtime: {{...}}, pinned at {{version}}.
- {{Board/device constraints worth a rule.}}

## 7. Concurrency

| Task/worker | Prio | Job |
|-------------|:----:|-----|
| {{...}} | {{...}} | {{...}} |

- {{Locking rules; which primitives guard what.}}
- {{Interrupt/handler rules if applicable — e.g. callbacks copy or enqueue, never process.}}

## 8. Fault Isolation

- A failing module returns its error code; it never halts the system.
- {{Watchdog/reset policy.}}

## 9. Logging

- {{What's logged, at what level, in what format — e.g. one strict line per event, no free prose on the hot path.}}
