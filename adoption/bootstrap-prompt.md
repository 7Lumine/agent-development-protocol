# Bootstrap Prompt for a New Project

Give this to a fresh ORCH session when adopting the protocol into an existing repository.

```text
Adopt the Agent Development Protocol into this repository without changing product behavior.

Goals:
1. Inspect the actual repository, current development docs, commands, CI, deployment model, and agent/tool environment.
2. Separate universal protocol rules from project-specific reality.
3. Install or adapt:
   - development lifecycle
   - orchestrator contract
   - Task Contract / Evidence / Findings templates
   - one role/model routing source of truth
   - a short AGENTS.md entry point
   - a project-profile document containing official commands, risk triggers, environment facts, and release gates
4. Remove or redirect duplicated/stale process sources of truth rather than keeping conflicting copies.
5. Do not change product code, DB/API/UI behavior, deployment scripts, or business logic as part of adoption.

Important constraints:
- Treat the repository as source of truth; verify claims from actual files/code/configuration.
- Do not blindly copy project-specific examples from another repository.
- Do not make every task Critical.
- Do not add heavy evidence, SHA-256, mutation, or review phases without a concrete project need.
- Keep role/model/provider/effort configuration in one routing source of truth.
- Keep runtime capability separate from role permission; a reviewer may have broad execution capability while tracked-file edits remain prohibited.
- Preserve the two-round review convergence rule.
- Repository artifacts, not chat history, are long-term memory.

Deliverables:
- list of files added/changed
- project-specific profile
- explicit Low/Normal/Critical route
- official verification gates
- role routing
- stale/conflicting rules removed or redirected
- remaining unknowns
- before/after workflow

After implementation, perform one independent process review scoped to the adoption diff. That review should check contradictions and broken references, not redesign the protocol.
```

## Recommended first live trial

After adoption, test the process on 2-3 real tasks before changing the protocol again:
- one Low/Normal task
- one Critical or state-bearing task if the project has one

Record friction and change the protocol only when the same problem is observed in practice, rather than optimizing hypothetically.