# Model Routing Example

This is an example routing policy, not a universal requirement. Each project should keep its live model/provider/effort/backend/runtime-access mapping in one role-routing source of truth and reference roles elsewhere.

The Project Profile records what execution capabilities/limitations actually exist. The router chooses among those capabilities for each role. Role permissions such as tracked-file edit rights are separate from runtime capability.

## Current example

| Role | Example model | Effort | Runtime access | Role permission |
|---|---|---:|---|---|
| ORCH / Decision | Claude Opus 5 | high | project-defined | may edit process/task artifacts when the route requires it |
| ORCH simple control | Claude Opus 5 | medium optional | project-defined | task-control edits as allowed |
| spec-review / Normal | Codex Sol 5.6 | high | full-capability environment | tracked-file edits prohibited |
| spec-review / Critical | Codex Sol 5.6 | xhigh | full-capability environment | tracked-file edits prohibited |
| implementation | Codex Luna | max | full access | edits allowed only within the frozen Task Contract |
| code-review / Normal | Codex Sol 5.6 | high | full-capability environment | tracked-file edits prohibited |
| code-review / Critical | Codex Sol 5.6 | xhigh | full-capability environment | tracked-file edits prohibited |
| verify | lightweight model | task-dependent | enough to run required checks | mechanical evidence only; no design changes |
| advisor-design | Claude Opus 5 | xhigh | project-defined | tracked-file edits prohibited |

## Policy

- Opus/high is the normal decision/orchestration baseline.
- Sol/high is the normal review baseline; Critical review may use xhigh.
- Luna/max is the implementation default in this example because implementation begins only after direction, scope, and acceptance criteria are frozen.
- Do not use xhigh/max merely to keep an old session alive.
- Do not scatter concrete model/effort/provider/runtime-access names across contracts and process documents. Keep one routing source of truth.
- If a model becomes unavailable, preserve the role requirements first: independence, permissions, context, and review scope matter more than a particular brand/model name.

## Capability vs runtime routing vs permission

These are three distinct layers:

1. **Project Profile:** what the actual environment can and cannot do.
2. **Role router:** which model/backend/runtime access a role should receive.
3. **Role contract:** what that role is permitted to edit or change.

A reviewer may need a full-capability environment to execute read-only diagnostics while still being forbidden from modifying tracked files.
Do not treat a restrictive sandbox as a substitute for a clear role contract if that sandbox prevents required verification.
