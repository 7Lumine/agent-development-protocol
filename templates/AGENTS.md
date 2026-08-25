# AGENTS.md Template

> Copy and adapt this file into the adopting repository. Keep it short. Do not duplicate the lifecycle here.

## Source of truth

Development work follows:
- project lifecycle: `<path to adopted development-lifecycle.md>`
- orchestrator contract: `<path to orchestrator-contract.md>`
- project profile: `<path to project-profile.md>`
- role/model router: `<path to routing source of truth>`

## Repository rules

- Default branch: `<main/master/...>`
- Branch/worktree policy: `<rules>`
- Push policy: `<for example: push only with explicit user permission>`
- Allowed git operations for implementers: `<rules>`
- Allowed git operations for reviewers: read-only operations unless explicitly authorized

## Access policy

Runtime capability and edit permission are separate.
- implementation role: `<access>`; tracked edits only within the contract's Allowed files
- review roles: `<access>`; tracked-file edits prohibited by role prompt/policy
- verification role: `<access>`; no design changes

## Official verification gates

These are the project-specific source of truth. Record exact commands and working directories.

| Gate | Command | cwd | When required |
|---|---|---|---|
| lint | `<command>` | `<cwd>` | `<rule>` |
| tests | `<command>` | `<cwd>` | `<rule>` |
| typecheck | `<command>` | `<cwd>` | `<rule>` |
| build | `<command>` | `<cwd>` | `<rule>` |
| other | `<command>` | `<cwd>` | `<rule>` |

Do not claim a gate passed using a narrower substitute command.

## Environment facts

Only durable, verified facts belong here. Examples:
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