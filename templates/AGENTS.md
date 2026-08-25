# AGENTS.md Template

> Copy and adapt this file into the adopting repository. Keep it short. Do not duplicate the lifecycle, official verification commands, or concrete role-routing values here.

## Source of truth

Development work follows:
- project lifecycle: `<path to adopted development-lifecycle.md>`
- orchestrator contract: `<path to orchestrator-contract.md>`
- project profile: `<path to project-profile.md>` — canonical project facts and official verification gates
- role/model router: `<path to routing source of truth>` — canonical model / effort / backend / runtime-access selection

Role permissions such as tracked-file edit rights come from the lifecycle/router role contract, not from runtime capability.

## Repository rules

- Default branch: `<main/master/...>`
- Branch/worktree policy: `<rules or link to project profile>`
- Push policy: `<for example: push only with explicit user permission>`
- Allowed git operations for implementers: `<rules>`
- Allowed git operations for reviewers: read-only operations unless explicitly authorized

## Runtime capability vs permission

Runtime capability and edit permission are separate concerns.

- Available capabilities and limitations are recorded in the project profile.
- Role-specific runtime access is selected by the role router.
- Tracked-file edit permissions are defined by the role contract and current Task Contract.

Do not duplicate concrete access values here.

## Official verification gates

The canonical command/cwd table lives in the project profile. Link to it here:

`<path to project-profile.md#official-verification>`

Agents must run the applicable canonical gates and must not claim a gate passed using a narrower substitute unless the project profile explicitly permits it.

## Environment facts

Keep only short entry-point facts or links here. Durable project reality belongs in the project profile. Examples:
- package/runtime manager
- virtualenv/toolchain location
- database/service requirements
- CI availability
- worktree-specific caveats

Do not preserve temporary incident history here.

## Project-specific prohibitions

- `<destructive commands not allowed>`
- `<directories not to touch>`
- `<production actions requiring user approval>`

## Handoff

Long-term state belongs in repository artifacts, not chat memory. A fresh session should be able to resume from the task contract/evidence/findings.
