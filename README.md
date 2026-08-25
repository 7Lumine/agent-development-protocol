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
- risk-scaled state-model requirements
- `SPEC_UNDEFINED` instead of implementer guesswork
- independent review with explicit Blocker criteria
- two-round review convergence (`R1` broad, `R2` closure)
- finding classification and routing
- evidence proportional to risk
- clear separation between environment capability, runtime routing, and role permission
- one-source-of-truth model/provider/effort/backend/runtime-access routing
- immutable frozen-contract identity and explicit candidate/review-target identity

## Repository layout

```text
protocol/
  development-lifecycle.md       Common lifecycle source of truth
  orchestrator-contract.md       ORCH responsibilities and session lifetime
  model-routing.example.md       Example model/effort/runtime-access policy

templates/
  AGENTS.md                       Short repository entry-point template
  project-profile.md              Project-specific reality/profile template
  task/
    contract.md                  Immutable after freeze for a contract version
    evidence.md                  Mutable task phase/review identity/evidence
    findings.md                  Mutable findings/routing/closure

integrations/
  claude/codex-task/SKILL.md      Generic role-router Skill template

adoption/
  bootstrap-prompt.md             First-time adoption into a repository
  migration-guide.md              Reconcile/replace an existing agent workflow
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
- tool/runtime capabilities and limitations
- model/provider IDs if pinned
- deployment/release/rollback gates
- project-specific invariants

See [`templates/project-profile.md`](templates/project-profile.md).

## Source-of-truth ownership

Keep each concern in one place:

- **Project Profile:** verified project facts, official verification commands/cwd, available runtime capabilities/limitations
- **Role router/config:** role -> model / effort / backend / runtime-access selection
- **Role contract + Task Contract:** what a role may edit/do for the task
- **AGENTS.md:** short entry point and links only; do not duplicate the command/access tables
- **Task Contract:** specification and guarantee boundary; immutable once frozen for a contract version
- **Evidence:** mutable task phase and baseline/frozen/candidate/review-target SHAs
- **Findings:** mutable review findings, dispositions, routing, and closure

Runtime capability and edit permission are different. A reviewer may use a broad-capability runtime while still being forbidden from editing tracked files.

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

### First-time / greenfield adoption

Use [`adoption/bootstrap-prompt.md`](adoption/bootstrap-prompt.md) when the repository does not yet have a substantial Agent Development Protocol. This can be a greenfield project or an existing codebase with little/no agent workflow.

### Existing workflow migration

Use [`adoption/migration-guide.md`](adoption/migration-guide.md) when the repository already has multiple process documents, agent rules, review queues, or routing rules that must be reconciled rather than simply added.

In either case, the adoption ORCH should inspect the actual repository first and populate project-specific rules rather than blindly copying another project's environment assumptions.

## Let an AI agent install the protocol

You do not need to copy the files manually. If your coding agent can read both this repository and the target project, give it this repository URL and ask it to adopt the protocol into the target repository.

The agent should treat the **target repository as the source of truth for project reality** and this repository as the source of truth for the **common development protocol**. It must adapt project-specific commands, risk triggers, runtime constraints, release rules, and routing instead of copying assumptions from another project.

### Minimal prompt

```text
Adopt the Agent Development Protocol from:
https://github.com/7Lumine/agent-development-protocol

Inspect this target repository as the source of truth and apply the protocol appropriately.
Use adoption/bootstrap-prompt.md for first-time adoption, or adoption/migration-guide.md if an existing agent/development workflow must be reconciled.
Do not change product behavior as part of protocol adoption.
```

For many projects, this is enough when the agent has repository access and can inspect the current development setup itself.

### Recommended prompt

Use this version when you want the adoption boundary to be explicit:

```text
Adopt the Agent Development Protocol from:
https://github.com/7Lumine/agent-development-protocol

Target repository: <owner/repository>

Requirements:
1. Treat the target repository as the source of truth for its current code, tooling, CI, deployment/release process, and existing agent rules.
2. Read the protocol repository before editing the target repository.
3. Decide whether this is first-time adoption or migration of an existing workflow, then follow the corresponding adoption guide.
4. Create/adapt the Project Profile from verified project facts, including official verification commands/cwd, Critical-risk triggers, environment capabilities/limitations, and real-environment/release gates.
5. Establish one role-routing source of truth for model / effort / backend / runtime access. Do not duplicate concrete routing values across task contracts or process documents.
6. Keep runtime capability separate from role permission. Review roles may have broad execution capability while tracked-file edits remain prohibited.
7. Keep AGENTS.md short and use it as an entry point to the lifecycle, Project Profile, router, and task artifacts rather than duplicating those sources of truth.
8. Preserve Low / Normal / Critical risk scaling; do not make every task Critical.
9. Do not add extra review phases, mutation requirements, SHA-256 checks, artifacts, or hardening without a concrete project need.
10. Do not change product code, DB/API/UI behavior, business logic, or deployment behavior merely to adopt the protocol.
11. After adoption, perform one independent process review scoped to the adoption diff. Review contradictions, broken references, duplicate sources of truth, routing/access conflicts, and migration accuracy; do not redesign the protocol from scratch.

Report:
- files added/changed
- Project Profile decisions and remaining unknowns
- official verification source of truth
- role-routing source of truth
- Low / Normal / Critical routes
- existing rules removed, redirected, or preserved
- independent review result
- git/branch/push state
```

### What the adopting agent should produce

A successful adoption normally leaves the target repository with:

- a short `AGENTS.md` entry point
- a project-specific Project Profile
- the adopted lifecycle and ORCH contract, or clearly versioned references to them
- Task Contract / Evidence / Findings templates
- one live role-routing source of truth
- no conflicting duplicate process rules
- an adoption diff that does not change product behavior

For an existing repository with substantial agent rules, the agent should **reconcile** them rather than stacking a second workflow on top. For a greenfield project, it should keep the initial setup minimal and let the protocol grow only when real project needs justify it.

If this protocol repository is private, the adopting agent must have GitHub access to it.

## Review identity

For SHA-based review, do not assume branch/PR HEAD is the target.

- frozen contract SHA identifies the immutable contract version
- implementation candidate SHA identifies the code candidate
- later metadata commits may update Evidence/Findings without becoming a new candidate
- a reviewer confirms the recorded review-target SHA and does not silently move it when HEAD differs

## Design constraints

This protocol deliberately avoids several common failure modes:

- using one giant ORCH session as project memory
- asking review to finish an incomplete specification
- treating every task as Critical
- letting reviewers invent stronger requirements as Blockers
- using higher model effort as a substitute for a clear contract
- repeatedly starting broad third/fourth/fifth reviews instead of diagnosing non-convergence
- scattering concrete model/effort/provider/runtime-access choices through every task document
- duplicating official verification command tables across Profile and AGENTS
- conflating runtime capability with tracked-file permission
- making task metadata edits silently move a SHA-fixed review target
- adding SHA-256, mutation suites, or audit artifacts without a concrete failure mode
