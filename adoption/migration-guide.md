# Migration Guide

Use this when extracting the protocol from an existing project or replacing an older multi-document agent workflow.

For a repository that does not yet have a substantial agent workflow, start with [`bootstrap-prompt.md`](bootstrap-prompt.md).

## 1. Inventory before editing

Identify:
- current process sources of truth
- AGENTS/README/CONTRIBUTING rules
- task/review queues
- model/provider routing rules
- official verification commands
- CI reality
- deployment/release gates
- project-specific safety invariants
- stale or contradictory rules

Do not assume that the newest-looking document is authoritative; verify what the repository and actual tooling use.

## 2. Separate common protocol from project facts

Move to the common protocol only rules that remain true across projects:
- Low/Normal/Critical assurance model
- Task Contract / evidence / findings split
- ORCH / implementer / reviewer role separation
- `SPEC_UNDEFINED`
- frozen-contract implementation
- finding classification/routing
- two-round review convergence
- repository-as-memory
- capability vs permission separation

Keep project-specific:
- exact commands/cwd
- framework/toolchain quirks
- CI status
- DB/runtime requirements
- deployment procedures
- project-specific Critical triggers
- production/manual gates
- concrete model/provider IDs if the project chooses to pin them

## 3. Establish one source of truth per concern

Recommended ownership:
- lifecycle: one development-lifecycle document
- ORCH behavior: one orchestrator contract
- **official verification commands/cwd and environment capability facts: one Project Profile**
- **model/provider/effort/backend/runtime-access selection: one role router/config**
- **role/task edit permissions: role contract + Task Contract**
- AGENTS: short entry point that links to the sources above; do not duplicate command or runtime-access tables
- immutable task specification: per-task `contract.md`
- mutable phase/review identity/evidence: per-task `evidence.md`
- findings and closure state: per-task `findings.md`
- review queue: active index only, not permanent process history

Replace obsolete documents with short transfer notices if links/history require them to remain.

## 4. Preserve immutable review identities

Do not mix frozen specification identity with mutable task metadata.

For an active task:
1. record the current baseline
2. reconstruct the current accepted contract/guarantee boundary
3. freeze the contract as a specific commit SHA
4. record that frozen SHA in Evidence, not by editing the frozen contract after the fact
5. record implementation candidate/review-target SHA in Evidence
6. treat later evidence/findings metadata commits as metadata only; do not silently move the review target to branch/PR HEAD
7. preserve unresolved findings
8. enter the next lifecycle phase under the new naming (`S-R1`, `I-R1`, etc.)

Do not force old review-round numbering into the new lifecycle, and do not silently erase old findings just because the process changed.

## 5. Review the process adoption once

Use one independent reviewer to check:
- conflicting sources of truth
- broken links/section references
- duplicated routing values
- access-policy contradictions
- incorrect SHA labels or review-target ambiguity
- active-task migration accuracy

A closure pass may verify those findings. Do not recursively redesign the process during the closure review.

## 6. Trial before further optimization

Run the adopted process on real tasks before introducing more roles, artifacts, or review phases.
If a friction point occurs once, record it. If it recurs and materially harms delivery or correctness, improve the common protocol or project profile at the appropriate layer.
