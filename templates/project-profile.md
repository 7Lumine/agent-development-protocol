# Project Profile

This file contains only project-specific facts that customize the common Agent Development Protocol.

## Project

- Name:
- Repository:
- Default branch:
- Primary stack:
- Deployment model:

## Official verification

| Gate | Command | cwd | Required for |
|---|---|---|---|
| lint |  |  |  |
| tests |  |  |  |
| typecheck |  |  |  |
| build |  |  |  |
| migration/other |  |  |  |

## Repository and worktree rules

- Branch policy:
- Worktree policy:
- Push policy:
- Merge policy:
- CI status/capabilities:

## Agent execution environment

- Available agent backends/providers:
- Full-access capability:
- Known sandbox/tooling limitations:
- Required local services:
- Required credentials or user-controlled actions:

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
- Areas where a reviewer may execute tests but may not edit:
- Known false-positive/over-engineering traps:

## Project-specific non-goals

Things the common protocol must not force on this project:

- 
