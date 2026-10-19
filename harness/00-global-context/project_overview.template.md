<!-- Template: project overview — tier 0. Edit points: everything. Keep it
     under ~160 lines; this loads into every agent session. -->

# Project Overview: {{PROJECT}}

{{One paragraph: what it is, who uses it, what it replaces.}}

{{ASCII architecture diagram — register it in the chart registry:
`<!-- chart-id: CH-overview-01 rev1 -->` above the fence.}}

```
{{ARCHITECTURE DIAGRAM}}
```

## Data flow

{{Numbered walk of one unit of data through the system, in prose or a
second diagram.}}

## Responsibility split

| Concern | Where it lives | Why |
|---------|----------------|-----|
| {{concern}} | {{device/host/module}} | {{one line}} |

## Scope

**v1 in:** {{bullets}}
**v1 out:** {{bullets — explicit non-goals are as valuable as goals}}

## Feature summary

| Feature | One-line result | Spec |
|---------|-----------------|------|
| {{...}} | {{...}} | `01-features/{{...}}/feature_{{...}}.md` |

## Hardware / platform

| Item | Choice | Notes |
|------|--------|-------|
| {{MCU/board}} | {{...}} | {{...}} |

## Library Reuse Policy (MANDATORY)

Before writing any module, check the platform's built-ins and package
registry first. Only build in-house when the built-in fails a stated
requirement. Every such decision gets one line here:

| Need | Built-in tried | Verdict | Replacement |
|------|----------------|---------|-------------|
| {{...}} | {{...}} | {{kept / rejected because …}} | {{...}} |
