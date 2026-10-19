# Ignore-file strategy for agent contexts

Agents load the repo into their context window — unmanaged, they swallow
archives, vendored code, and scratch logs. Two mechanisms keep context clean.

## 1. The two-zone ignore file (`.zcodeignore` style)

If your agent supports an ignore file, structure it in two zones:

```
# ===== project ignores (synced from .gitignore — rewrite only this part) =====
build/
*.log
{{secrets file}}
{{scratch dirs}}

# ===== ↑ end synced zone =====
{{agent-tool default exclusions}}
# custom rules below this line (never auto-overwritten)
```

- Top zone mirrors `.gitignore` so build artifacts never enter context.
- Bottom zone carries agent-specific exclusions (session dirs, plan files).
- Keep `.gitignore` itself listing the secrets file (`{{env file}}`) —
  **the ignore file is committed, so it must never contain secrets, only
  their names.**

## 2. Tiered auto-loading by banner

More important than exclusion is *inclusion order*. Wire the agent's
context loading to this tier scheme:

| Tier | Path | Auto-loaded? | Content |
|------|------|--------------|---------|
| 0 | root agent files (`AGENTS.*.md`, `CLAUDE.md`) | always | constitution: rules, layout, build |
| 1 | `harness/00-global-context/` | always | spec, coding rules, workflow, privacy |
| 2 | `harness/01-features/<current stage>/` | on task match | per-feature spec + acceptance criteria |
| 3 | `harness/02-knowledge/` | on demand | process reports, run transcripts |
| 4 | `harness/02-future/` | **never** | parking lot — mark its README with a "NOT auto-loaded as LLM context" banner |
| 5 | archives, vendored code, `docs/templates/` | never | frozen point-in-time material |

The `harness/02-future/README.md` banner pattern ("this folder is NOT
auto-loaded") prevents the most common context pollution: speculative
designs being treated as current truth.
