# Development Lifecycle

This document is the single source of truth for the reusable development process.
Project-specific commands, environments, risks, and release rules belong in the project's `project-profile.md` or equivalent repository documentation.

## 1. Principles

1. Complete the specification before asking review to complete it.
2. Allocate assurance by risk; both under-testing and over-engineering are defects.
3. Higher model effort is not a substitute for fresh context, role separation, a frozen contract, and a bounded review scope.
4. The repository is long-term memory. Decisions needed by future sessions must not live only in chat history.
5. A reviewer falsifies the declared contract; a reviewer does not invent a stronger product.
6. One contract equals one task and one review unit. Do not mix unrelated concerns.

## 2. Risk classes

### Low

Typical examples: copy, CSS, documentation, small local changes, or simple changes well-covered by existing tests.

Default assurance:
- implementer self-check
- diff review
- no independent review unless there is a concrete reason
- no mutation testing or SHA-256 merely for ceremony
- contract optional or very small

### Normal

Typical examples: ordinary feature work, non-destructive API/UI changes, multi-file implementation, normal synchronization or aggregation.

Default assurance:
1. short Task Contract
2. implementation
3. official project verification
4. one independent code review
5. one closure pass if needed
6. ORCH integration decision

Independent spec review is optional for Normal work.

### Critical

Projects define their own Critical triggers in `project-profile.md`. Typical generic triggers include:
- migrations or persistent-state format changes
- deployment, restart, recovery, rollback
- concurrency, locks, duplicate prevention
- destructive operations or production data correction
- authentication/authorization
- retry/backoff or failure-state machines
- external side effects
- new executable scripts or automation
- cancellation/restoration state machines

Default assurance:
1. ORCH Reality Check + draft Task Contract
2. pre-implementation spec review
3. ORCH finding routing and contract correction
4. contract freeze
5. implementation
6. implementer official self-verification
7. mechanical evidence audit when useful
8. independent candidate-SHA code review
9. ORCH release/integration decision
10. project-specific deployment or real-environment gates where required

Code-review approval is not automatically production-activation approval.

## 3. Task phases

```text
draft -> spec-review -> frozen -> implementation -> code-review -> accepted
```

A project may add a deployment/activation phase after `accepted` if its release model needs one.

Keep task state in repository artifacts such as:

```text
docs/tasks/<TASK-ID>/
  contract.md
  evidence.md
  findings.md
```

Do not keep these only in chat:
- specification decisions
- undefined decisions
- baseline / frozen contract / candidate SHAs
- unperformed verification
- blockers
- production or real-environment facts
- rollback criteria

## 4. Reality Check

Before writing a Critical contract, verify the relevant reality instead of inferring it from intent.
Depending on the project, inspect:
- actual code paths and upstream branches
- schema and persistent data shape
- process/runtime topology
- configuration and PATH/tooling
- deployed version and legacy state
- external-service boundaries
- rollback reality

For existing behavior, cite the actual code/config/schema source that proves the statement.

## 5. State-bearing changes

If a change introduces or modifies state, write a state model or transition table before implementation.
This includes status/progress, confirm/decline, retry/pending/completed, migration states, cancellation/restoration, or any behavior whose outcome depends on persisted/current state.

At minimum describe:
- starting state
- input/decision/event
- relevant current persistent state
- concurrent/intermediate changes that may occur
- expected result
- persistence updates
- side effects
- externally visible result
- test/evidence that fixes the behavior

Rules:
- do not implement while a material state cell is undefined
- model the state newly introduced by the change, not only existing code branches
- when multiple fields encode state, enumerate valid combinations and invalid combinations
- keep independent concerns in separate state groups
- if the state model keeps growing and review does not converge, reconsider the guarantee boundary rather than adding state indefinitely

## 6. Undefined specification during implementation

The implementer must not invent missing product decisions. Return:

```text
SPEC_UNDEFINED
- question:
- why needed:
- affected behavior:
- affected files:
- current safe stopping point:
```

ORCH routes this back to the contract.

## 7. Verification and evidence

Each project defines its official verification commands in its project profile or `AGENTS.md`.
The implementer runs the applicable official checks; "the reviewer will run them later" is not a reason to skip them.

Evidence is proportional to risk.
For Critical tasks, normally record:
- baseline / frozen-contract / candidate SHA
- changed files
- working-tree state
- exact commands and working directories
- material environment conditions
- pass/fail/skip results
- real path vs stub/simulation distinctions
- mutation/fault-injection results when used, including the assertion that failed
- unperformed checks
- real-environment verification status

Do not treat a bare `passed` claim as sufficient evidence.

### SHA usage

- Git-managed identity: commit SHA
- frozen spec: commit SHA containing the frozen contract, optionally with a contract version
- candidate: candidate commit SHA
- SHA-256 is not a standard ritual; use it only for genuine bit-level integrity or restore-verification needs

### Mutation/fault injection

Mutation testing answers whether an existing test detects a changed condition. It does not discover specification states that were never tested.
Use this order:
1. enumerate states/spec cells
2. ensure tests/evidence map to the important cells
3. mutate only high-value conditions when useful

Do not automatically classify every surviving mutation as a defect; diagnose equivalent/redundant guards first. Do not automatically dismiss it as equivalent either.

## 8. Review contract

Review the declared diff/candidate and its direct dependencies. Do not perform unbounded surrounding exploration.

A Blocker or High finding must include:
1. violated requirement/invariant
2. concrete reachable failure mode
3. minimum correction
4. finding classification
5. introduced-by-change vs pre-existing

Normally non-blocking:
- stronger guarantees not requested by the contract
- future-only abstraction
- extra state/helper/audit layers
- "just in case" large mutation suites
- SHA-256 without an integrity need
- general hardening outside the task goal

### Finding classes

Use these before routing:
- `implementation defect`
- `specification defect`
- `pre-existing defect`
- `evidence defect`
- `review/process defect`
- `review scope expansion`
- `non-blocking suggestion`

Suggested routing:
- implementation defect -> implementer
- specification defect -> ORCH / contract
- pre-existing defect -> separate task unless it directly blocks the current task
- evidence defect -> implementer/verifier
- review/process defect -> ORCH / process owner
- review scope expansion -> backlog by default
- non-blocking suggestion -> ORCH cost/benefit decision

## 9. Review convergence

For one frozen contract, use at most two normal review rounds.

### R1
Broad review:
- contract compliance
- important regressions
- direct dependencies
- meaningful scope/behavior problems

### R2
Narrow closure review:
- R1 finding closure
- changed locations
- regressions directly caused by the fix

After R2, do not automatically start R3. Diagnose:
- specification gap
- architecture issue
- scope too large
- implementation strategy problem
- acceptance-criteria error
- moving review target

The same rule applies to spec review (`S-R1` broad, `S-R2` closure).
If a fix introduces a new persistent state, lock, executable, external side effect, rollback stage, or other important state after freeze, version/refreeze the contract and return to spec review.

Do not force convergence by increasing effort to the maximum. Use a short advisor session to diagnose why the task is not converging.

## 10. Session lifetime and handoff

ORCH sessions should normally be one task or one phase, not permanent project memory.
Before compaction, session limits, or moving to another session, make the repository sufficient for resumption.

A handoff should be reconstructible from:
1. task ID and contract status
2. baseline / frozen contract / candidate SHAs
3. unresolved findings
4. next lifecycle phase
5. user-decision items

If these are in repository artifacts, a long chat summary is optional.

## 11. Project profile

Every adopting project must explicitly define:
- official verification commands and required working directories
- repository/branch/worktree rules
- Critical-risk triggers specific to the project
- deployment/release/rollback rules
- real-environment gates
- allowed agent backends/providers
- access policy
- project-specific invariants and prohibited changes

The common protocol does not override project reality. If generic guidance conflicts with verified project constraints, update the project profile and keep the common lifecycle generic.