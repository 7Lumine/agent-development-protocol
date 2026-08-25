# Project Profile

This file is the canonical source of truth for project-specific reality that customizes the common Agent Development Protocol.

It owns **official verification commands** and **available execution capabilities/limitations**. It does **not** assign role-specific model/effort/backend/runtime access; those choices belong to the project's single role router. Role permissions such as tracked-file edit rights belong to the role contract, not this profile.

## Project

- Name:
- Repository:
- Default branch:
- Primary stack:
- Deployment model:

## Official verification — canonical source of truth

Record the exact commands, working directories, and applicability rules here. `AGENTS.md` and task prompts should reference this section rather than duplicate command tables.

| Gate | Command | cwd | Required for |
|---|---|---|---|
| lint |  |  |  |
| tests |  |  |  |
| typecheck |  |  |  |
| build |  |  |  |
| migration/other |  |  |  |

Do not claim a gate passed using a narrower substitute command unless this profile explicitly permits it.

## Repository and worktree rules

- Branch policy:
- Worktree policy:
- Push policy:
- Merge policy:
- CI status/capabilities:

## Agent execution environment — capabilities and limitations

Record facts about what the environment can do. Do not assign role-specific runtime access here.

- Available agent backends/providers:
- Available execution capabilities:
- Network/filesystem/runtime limitations:
- Known sandbox/tooling limitations:
- Required local services:
- Required credentials or user-controlled actions:

The role router chooses model / effort / backend / runtime access from within these verified capabilities. A role contract separately defines what the agent is permitted to modify.

## Critical-risk triggers

Start from the generic lifecycle and add/remove triggers based on this project.

- [ ] migration / persisted format change
- [ ] deploy / stop / restart / recovery / rollback
- [ ] concurrency / locks / duplicate prevention
- [ ] destructive or production-data operations
- [ ] auth / permission
- [ ] retry / backoff / failure state machine
- [ ] external side effect
- [ ] new executable automation
- [ ] project-specific:

## Global safety / correctness invariants

Long-lived invariants that apply across tasks:

1.
2.

## Real-environment gates

What cannot be proven by repository tests alone?

- production/staging/manual verification:
- canary requirements:
- activation approval:
- rollback verification:

## Project-specific review constraints

- Areas requiring independent review:
- Areas where runtime limitations affect verification:
- Known false-positive/over-engineering traps:

## Project-specific non-goals

Things the common protocol must not force on this project:

- 
