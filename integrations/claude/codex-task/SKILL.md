---
name: codex-task
description: Role-based development-task router. Selects spec-review, implementation, code-review, verify, or advisor-design according to the adopted lifecycle and project profile. Concrete provider/model IDs are project configuration, not protocol law.
---

# Role-based Task Router

Read the adopting project's:
1. development lifecycle
2. orchestrator contract
3. project profile / AGENTS
4. live model-routing source of truth

Do not infer project commands, risk, or access rules from this generic file.

## Routing procedure

1. classify the task as Low / Normal / Critical
2. choose the required role for the current phase
3. ensure the required task artifact exists
4. resolve model / effort / backend / access from the project's single routing table
5. launch the role with a bounded prompt
6. classify returned findings before routing them
7. preserve the two-round convergence rule

## Role contracts

### `role=spec-review`

Input:
- draft contract
- baseline SHA
- specific questions if any

Purpose:
- falsify the declared contract
- identify reachable contradictions and missing material states

Blocker/High output must include:
- violated requirement/invariant
- concrete reachable failure mode
- minimum correction
- classification
- introduced vs pre-existing

Prohibited:
- tracked-file edits
- inventing stronger requirements as blockers
- unbounded surrounding review

### `role=implementation`

Input:
- frozen contract SHA/version
- Allowed files
- Forbidden changes
- official verification gates

Behavior:
- implement the frozen contract
- do not widen scope
- return `SPEC_UNDEFINED` instead of inventing missing requirements
- run applicable official verification before reporting completion

Prohibited:
- changing frozen requirements
- unrelated hardening
- push/merge unless explicitly authorized by the project/user

### `role=code-review`

Input:
- frozen contract
- candidate SHA
- evidence artifact

Purpose:
- test whether candidate satisfies the frozen contract and important direct dependencies

Prohibited:
- tracked-file edits
- new product requirements
- treating pre-existing issues as current blockers unless directly blocking the task

### `role=verify`

Input:
- exact commands/checks
- expected results
- environment/cwd

Purpose:
- mechanical evidence collection only

Prohibited:
- design judgment
- opportunistic fixes

### `role=advisor-design`

Input:
- one bounded non-convergence or guarantee/architecture problem
- current contract/findings

Purpose:
- explain why the task is not converging and recommend a decision

Not a broad reviewer. Do not use it to accumulate new findings.

## Capability vs permission

A runtime may need broad capability for valid diagnostics while the role is still prohibited from modifying tracked files.
Keep access capability and role permission separate.
Do not hardcode one restrictive sandbox mode in this generic router.

## Session rules

- new task: new session/agent by default
- closure round for the same frozen target: reusing the same reviewer may be useful for continuity
- if the contract is materially redesigned/refrozen, a cold reviewer is appropriate
- do not keep sessions alive merely because they contain history; repository artifacts are the durable handoff
