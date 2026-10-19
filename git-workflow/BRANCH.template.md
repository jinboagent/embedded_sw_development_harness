<!-- Template: branch manifest — copy to the ROOT of every feature branch as
     BRANCH.{{AGENT_TAG}}.md at branch cut. Edit points: all {{...}} slots.
     Append-only after creation: update Status/ACs/Changelog as work lands;
     never delete history inside the file. -->

# Branch Manifest: {{branch-name}}

| Field | Value |
|-------|-------|
| Feature | {{one-line description}} |
| Spec | `harness/01-features/{{stage}}/feature_{{name}}.md` (or proposal doc) |
| Baseline | {{baseline branch}} @ {{cut commit sha}} |
| Cut date | {{YYYY-MM-DD}} |
| Status | {{active \| paused \| review-pending \| merged}} |

## Scope

**In:** {{bullets — what this branch will deliver}}
**Out:** {{bullets — adjacent work explicitly deferred}}

## Acceptance Criteria

- [ ] {{AC1 — observable, testable}}
- [ ] {{AC2}}
- [ ] {{AC3 — e.g. "hardware gate green on branch tip; transcripts filed"}}

## Verification Plan

| Claim | How proven | Where |
|-------|-----------|-------|
| {{feature works}} | {{suite/commands}} | `harness/02-knowledge/evidence-{{slug}}/` |

## Changelog (append-only)

- {{YYYY-MM-DD}} — {{milestone or decision; commit sha if landed}}
- {{YYYY-MM-DD}} — {{...}}

## Merge Record

- Squash commit: {{sha}} on {{baseline}} · merged {{date}} · FEATURES row: {{added/updated}}
