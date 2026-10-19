<!-- Template: feature-branch policy — the branch ritual that keeps
     multi-agent history readable. Edit points: baseline naming, branch
     prefix, the example rows. Source project ran this for months with
     multiple LLM agents; the manifest + index are what made merges
     auditable. -->

# Feature-Branch Workflow Policy

## Why

Agents (and humans) produce commits fast. Without structure, `{{main}}`
becomes an unauditable stream. This policy makes every landed feature
answer three questions mechanically: what was added (FEATURES index),
what was promised (BRANCH manifest), and what was proven (verification
section + evidence transcripts).

## Baseline registry

Branches are cut only from **registered baselines** — `{{main}}` being the
default. Register a baseline by recording it here:

| Baseline | Branch | Head sha | Registered | Notes |
|----------|--------|----------|------------|-------|
| {{B1}} | {{main}} | {{sha}} | {{date}} | {{e.g. "v1.0.0 feature-complete"}} |

## Feature-branch rules

1. Branch naming: `feature/<short-name>` (or `<stage>-<short-name>` if you
   use stages). One feature per branch.
2. Every feature branch carries a **root `BRANCH.{{AGENT_TAG}}.md`
   manifest** (template: `BRANCH.template.md`) — created at branch cut,
   updated as work lands, closed at merge.
3. The **squash-merge** lands one commit on the baseline; the commit
   message follows the 4-section standard.
4. At merge: update the root `FEATURES.{{AGENT_TAG}}.md` index (one row:
   feature / what it adds / branch + squash sha), **keep the branch**
   after merge (history + recovery), record the merge in the manifest.
5. Direct commits on `{{main}}` are owner-only; every such exception gets
   a line in the FEATURES "direct commits" log with the reason.

## Merge ritual checklist

- [ ] Manifest's acceptance criteria all checked, verification section filled
- [ ] Evidence transcripts exist under `harness/02-knowledge/` for every claim
- [ ] `{{hw gate / full test gate}}` green on the branch tip
- [ ] Squash-merge with 4-section message
- [ ] FEATURES index row added; branch kept; manifest marked merged
- [ ] Process report filed (`harness/02-knowledge/`)

## Agent pre-flight (before starting any feature)

1. `git status` clean; confirm current baseline from FEATURES header.
2. Read the matching spec under `harness/01-features/`; if none, write one
   (from `templates/feature_template.md`) or a proposal doc first.
3. Cut the branch; instantiate `BRANCH.{{AGENT_TAG}}.md` from the template.
4. Plan the verification before the code: which suite proves this feature?

## Retro-indexing

When an unregistered branch turns out to hold landed work: register the
baseline after the fact, rebuild the FEATURES rows from the git log, and
note the exception. Never rewrite `{{main}}` history to fit the policy.
