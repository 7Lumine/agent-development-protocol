# Model Routing Example

This is an example routing policy, not a universal requirement. Each project should keep its live model/provider/effort mapping in one place and reference roles elsewhere.

## Current example

| Role | Example model | Effort | Access | Tracked-file edits |
|---|---|---:|---|---|
| ORCH / Decision | Claude Opus 5 | high | project-defined | allowed for process/task artifacts |
| ORCH simple control | Claude Opus 5 | medium optional | project-defined | allowed |
| spec-review / Normal | Codex Sol 5.6 | high | full-capability environment | prohibited by prompt |
| spec-review / Critical | Codex Sol 5.6 | xhigh | full-capability environment | prohibited by prompt |
| implementation | Codex Luna | max | full access | allowed within contract |
| code-review / Normal | Codex Sol 5.6 | high | full-capability environment | prohibited by prompt |
| code-review / Critical | Codex Sol 5.6 | xhigh | full-capability environment | prohibited by prompt |
| verify | lightweight model | task-dependent | enough to run required checks | no design changes |
| advisor-design | Claude Opus 5 | xhigh | project-defined | prohibited |

## Policy

- Opus/high is the normal decision/orchestration baseline.
- Sol/high is the normal review baseline; Critical review may use xhigh.
- Luna/max is the implementation default in this example because implementation begins only after direction, scope, and acceptance criteria are frozen.
- Do not use xhigh/max merely to keep an old session alive.
- Do not scatter concrete model/effort/provider names across contracts and process documents. Keep one routing source of truth.
- If a model becomes unavailable, preserve the role requirements first: independence, edit permissions, context, and review scope matter more than a particular brand/model name.

## Capability vs permission

Runtime capability and role permission are separate.
A reviewer may need a full-capability environment to execute read-only diagnostics or project verification while still being forbidden by prompt/policy from modifying tracked files.
Do not treat a restrictive sandbox as a substitute for a clear role contract if that sandbox prevents the reviewer from performing required verification.