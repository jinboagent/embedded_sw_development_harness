# Skills — reusable agent skills extracted from the field

These are **agent skills** in the standard `SKILL.md` format: folders
with a YAML frontmatter (`name`, `description`) and instructions. An
agent that supports skills loads the description and invokes the skill
when a task matches. They are procedure distilled from a real
hardware+LLM project — each one was used, not imagined.

| Skill | What it does | Born from |
|---|---|---|
| `tmux-esp32-workflow` | Route ESP32 build/flash/monitor through a WSL tmux pane from a Windows agent (sentinel polling, log-tail reads) | daily firmware sessions |
| `chart-registry-governance` | Adopt + maintain an ASCII-diagram registry with a hash checker that fails on drift | the chart-registry system |
| `evidence-gate-builder` | Build a one-command regression gate: identity probe, subprocess suites, reset-reason checks, teed transcripts | the hardware gate |
| `matplotlib-diagrams` | Publication block diagrams as regenerable Python (PNG+SVG, width assertions, EN variants) | the docs/diagrams pipeline |
| `medium-article-pipeline` | Project → Medium-import-ready article page (import-safe elements, tables→bold-lead lists) | two published articles |
| `prepublish-audit` | The secrets/PII/history audit before anything goes public | the privacy audit ritual |

## Install (per person, per machine)

Skills are plain folders — copy the ones you want into your agent's
skills directory:

- **ZCode**: `~/.zcode/skills/<skill-name>/SKILL.md`
  (Windows: `%USERPROFILE%\.zcode\skills\...`)
- **Claude Code**: `.claude/skills/` (per project) or `~/.claude/skills/`
- **Cross-agent convention**: `.agents/skills/` in the repo

No build, no registration — the next agent session discovers them by
their frontmatter description. To verify, ask your agent "what skills
do you have?" or list the directory.

## Writing your own

Keep the shape of these: frontmatter description that says **when to
use** (trigger phrases), then *When to use / Procedure / Verification*.
The `description` is the only part the agent sees before invoking —
spend your words there. A skill worth keeping has a verification step;
a skill without one is a wish.
