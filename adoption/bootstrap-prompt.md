# Bootstrap Prompt

Use this with a fresh ORCH session to adopt the protocol into a repository that does not yet have this workflow. It works for greenfield repositories and existing codebases without an established Agent Development Protocol.

If the repository already has a substantial/multi-document agent workflow that must be reconciled or replaced, also use [`migration-guide.md`](migration-guide.md).

```text
Adopt the Agent Development Protocol into this repository without changing product behavior.

Goals:
1. Inspect the actual repository, current development docs, commands, CI, deployment model, and agent/tool environment.
2. Separate universal protocol rules from project-specific reality.
3. Install or adapt:
   - development lifecycle
   - orchestrator contract
   - Task Contract / Evidence / Findings templates
   - one role-routing source of truth for model / effort / backend / runtime access
   - a short AGENTS.md entry point
   - a Project Profile that is the canonical source for official verification commands/cwd, environment capabilities/limitations, risk triggers, project invariants, and release gates
4. Keep source-of-truth ownership explicit:
   - Project Profile = project facts, official verification, available execution capabilities/limitations
   - role router = role-specific model / effort / backend / runtime-access selection
   - role contract + Task Contract = edit/action permissions for the role/task
   - AGENTS.md = short entry point and links; do not duplicate command/access tables there
5. Remove or redirect duplicated/stale process sources of truth rather than keeping conflicting copies.
6. Do not change product code, DB/API/UI behavior, deployment scripts, or business logic as part of adoption.

Important constraints:
- Treat the repository as source of truth; verify claims from actual files/code/configuration.
- Do not blindly copy project-specific examples from another repository.
- Do not make every task Critical.
- Do not add heavy evidence, SHA-256, mutation, or review phases without a concrete project need.
- Keep role/model/provider/effort/backend/runtime-access configuration in one routing source of truth.
- Keep runtime capability separate from role permission; a reviewer may have broad execution capability while tracked-file edits remain prohibited.
- Keep official verification command/cwd definitions in the Project Profile only; AGENTS and prompts reference them.
- Preserve immutable frozen contracts: mutable phase/status and frozen/candidate/review-target SHAs belong in Evidence, not in the frozen contract file.
- Preserve the two-round review convergence rule.
- Repository artifacts, not chat history, are long-term memory.

Deliverables:
- list of files added/changed
- project-specific profile
- explicit Low/Normal/Critical route
- official verification gates in the Project Profile
- role routing
- stale/conflicting rules removed or redirected
- remaining unknowns
- before/after workflow

After implementation, perform one independent process review scoped to the adoption diff. That review should check contradictions and broken references, not redesign the protocol.
```

## Recommended first live trial

After adoption, test the process on 2-3 real tasks before changing the protocol again:
- one Low/Normal task
- one Critical or materially state-bearing task if the project has one

Record friction and change the protocol only when the same problem is observed in practice, rather than optimizing hypothetically.
