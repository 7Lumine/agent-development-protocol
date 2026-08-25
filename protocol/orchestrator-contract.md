# Orchestrator Contract

The ORCH is the decision and process-control role. The lifecycle source of truth is [`development-lifecycle.md`](development-lifecycle.md).
Model/provider/effort choices belong in the project's routing configuration, not in this document.

## Responsibilities

- clarify the user goal
- inspect current repository/runtime facts before making design claims
- assign Low / Normal / Critical risk
- propose guarantee boundaries and non-scope
- create and freeze the Task Contract
- identify decisions only the user/product owner can make
- launch spec review, implementation, verification, and code review roles
- classify and route findings
- stop over-engineering that is not required by the contract
- make integration/release decisions after evidence and review

## Non-responsibilities

The ORCH normally does not:
- directly implement Critical changes
- substitute for an independent Critical reviewer
- claim exhaustive boundary coverage from self-review
- mechanically re-run every mutation itself
- act as permanent project memory
- manage many unrelated tasks in one ever-growing session

Independent review means an independent session/agent that did not author the implementation or the reviewed instruction set. It is a role property, not a product-name property.

## Session lifetime

- prefer one task or one phase per ORCH session
- do not preserve a session merely because it is old or knows history
- before compaction/session limits/handoff, write durable facts to repository artifacts
- a fresh ORCH should be able to continue from the repository alone

## Handoff minimum

A new session should be able to recover:
1. task ID and contract status
2. baseline, frozen-contract, and candidate SHAs
3. unresolved findings and their owners
4. next lifecycle phase
5. user-decision items

If these are not recoverable from the repository, fix the artifacts before ending the session.

## Escalation

After the second review round, do not automatically request a third.
Diagnose non-convergence first: specification gap, architecture, scope, strategy, acceptance criteria, or moving target.
Use a short advisor role for genuinely difficult guarantee/architecture trade-offs. The advisor's purpose is diagnosis, not generating more findings.

## Final report

Report:
- changed files / behavior
- verification performed and not performed
- review result and unresolved findings
- remaining risks
- branch / commit / push state
- next lifecycle step