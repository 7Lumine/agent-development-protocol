# Agent Development Protocol

Reusable AI-agent development protocol for repository-based software projects.

The core idea is simple:

> **ORCH decides what to build and how much to guarantee. An independent reviewer tries to falsify that contract. An implementation agent codes only after direction and scope are frozen. The repository, not a long-running chat session, is the durable memory.**

This repository was generalized from a production development workflow, but project-specific runtime, deployment, and business rules are intentionally kept outside the common protocol.

## What this provides

- Low / Normal / Critical assurance routing
- ORCH / implementation / review role separation
- short-lived orchestration sessions and repository-based handoff
- Task Contract / Evidence / Findings artifacts
- state-model requirements for state-bearing changes
- `SPEC_UNDEFINED` instead of implementer guesswork
- independent review with explicit Blocker criteria
- two-round review convergence (`R1` broad, `R2` closure)
- finding classification and routing
- evidence proportional to risk
- clear separation between runtime capability and role permission
- one-source-of-truth model/provider/effort routing

## Repository layout

```text
protocol/
  development-lifecycle.md       Common lifecycle source of truth
  orchestrator-contract.md       ORCH responsibilities and session lifetime
  model-routing.example.md       Example model/effort policy

templates/
  AGENTS.md                       Short repository entry-point template
  project-profile.md              Project-specific reality/profile template
  task/
    contract.md
    evidence.md
    findings.md

integrations/
  claude/codex-task/SKILL.md      Generic role-router Skill template

adoption/
  bootstrap-prompt.md             Prompt for introducing the protocol to a repo
  migration-guide.md              Guide for moving an existing workflow
```

## Common vs project-specific

The protocol owns generic workflow rules.
Each adopting project owns its facts.

**Common:**
- roles
- risk/assurance structure
- contract freeze
- review convergence
- finding classification
- session/handoff rules

**Project-specific:**
- official test/lint/build commands and cwd
- branch/worktree rules
- Critical-risk triggers
- tool/runtime limitations
- model/provider IDs if pinned
- deployment/release/rollback gates
- project-specific invariants

See [`templates/project-profile.md`](templates/project-profile.md).

## Suggested role pattern

The model names are examples and can be replaced without changing the lifecycle.

```text
User / Product Owner
        |
        v
ORCH / Decision
        |
   +----+-------------------+
   |                        |
   v                        v
Spec Review             Design Advisor
   |                        |
   +-----------+------------+
               |
               v
       Frozen Task Contract
               |
               v
        Implementation
               |
               v
        Self-verification
               |
               v
      Independent Review
               |
               v
       ORCH release decision
```

A current example routing is in [`protocol/model-routing.example.md`](protocol/model-routing.example.md).

## Adoption

For an existing repository, start with [`adoption/bootstrap-prompt.md`](adoption/bootstrap-prompt.md).
The adoption ORCH should inspect the actual repository first and populate project-specific rules rather than blindly copying another project's environment assumptions.

For migration from an existing multi-document workflow, use [`adoption/migration-guide.md`](adoption/migration-guide.md).

## Design constraints

This protocol deliberately avoids several common failure modes:

- using one giant ORCH session as project memory
- asking review to finish an incomplete specification
- treating every task as Critical
- letting reviewers invent stronger requirements as Blockers
- using higher model effort as a substitute for a clear contract
- repeatedly starting broad third/fourth/fifth reviews instead of diagnosing non-convergence
- scattering concrete model/effort/provider choices through every task document
- adding SHA-256, mutation suites, or audit artifacts without a concrete failure mode

## Status

The initial generalized version is being prepared on `feat/initial-protocol` for review before merge to `main`.
