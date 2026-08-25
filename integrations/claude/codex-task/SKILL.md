---
name: codex-task
description: Role-based development-task router. Selects spec-review, implementation, code-review, verify, or advisor-design according to the adopted lifecycle and project profile. Concrete provider/model IDs are project configuration, not protocol law.
---

# Role-based Task Router

Read the adopting project's:
1. development lifecycle
2. orchestrator contract
3. project profile
4. live model-routing source of truth
5. short AGENTS entry point if present

Do not infer project commands, risk, or access rules from this generic file.

## Source-of-truth boundaries

- **Project Profile**: project facts, official verification commands/cwd, available runtime capabilities and limitations
- **Role router/config**: role -> model / effort / backend / runtime-access selection
- **Role contract + Task Contract**: what the role is permitted to edit/do for the current task

Runtime capability is not edit permission. Do not duplicate concrete runtime-access values in AGENTS or task contracts.

## Routing procedure

1. classify the task as Low / Normal / Critical
2. choose only the roles required by that Risk route and current phase
3. ensure the required task artifact exists
4. resolve model / effort / backend / runtime access from the project's single routing table, within capabilities verified by the Project Profile
5. launch the role with a bounded prompt
6. classify returned findings using the lifecycle's exact canonical classification strings before routing them
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
- classification using the lifecycle's canonical string
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
- official verification gates by reference to the Project Profile

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
- frozen contract SHA/version
- recorded implementation-candidate / review-target SHA from evidence
- evidence artifact

First action:
- resolve and confirm the requested review-target SHA
- if branch/PR HEAD differs, report that difference and **do not move the target** unless ORCH explicitly re-targets the review

Purpose:
- test whether the fixed candidate satisfies the frozen contract and important direct dependencies

Prohibited:
- tracked-file edits
- new product requirements
- treating pre-existing issues as current blockers unless directly blocking the task

### `role=verify`

Input:
- exact commands/checks from the Project Profile
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
Keep runtime-access selection and role permission separate.
Do not hardcode one restrictive sandbox mode in this generic router.

## Session rules

- new task: new session/agent by default
- closure round for the same frozen target: reusing the same reviewer may be useful for continuity
- if the contract is materially redesigned/refrozen, a cold reviewer is appropriate
- do not keep sessions alive merely because they contain history; repository artifacts are the durable handoff
