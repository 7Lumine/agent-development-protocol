# <TASK-ID> Evidence

Contract: [contract.md](contract.md)
Task phase: draft | spec-review | frozen | implementation | code-review | accepted
Last updated:

> This file holds mutable task state and review identity. Updating this metadata advances the repository HEAD but does **not** change the recorded frozen contract or implementation candidate identities.

## Review identity

| Type | SHA | Notes |
|---|---|---|
| baseline |  | comparison/reality-check origin |
| frozen contract |  | commit that contains the immutable frozen `contract.md` |
| implementation candidate |  | implementation commit to review |
| current review target |  | normally equal to implementation candidate for code review |

Rules:
- A metadata/artifact commit after the candidate is **not** automatically a new candidate.
- Reviewers must resolve and confirm the recorded review-target SHA themselves.
- If branch/PR HEAD differs from the recorded review target, report the difference and keep the requested target fixed unless ORCH explicitly re-targets the review.
- Never chase current HEAD merely because evidence/findings metadata was updated after the candidate commit.

## Changed files

<!-- Measured from git between the relevant baseline/frozen point and candidate, not only reported by the implementer. -->

## Verification performed

| Command | cwd | Environment | Result (pass/fail/skip) |
|---|---|---|---|

- real DB/runtime vs stub/simulation:
- worktree clean:

## Mutation / fault injection

| Mutation/fault | Assertion or observable that failed | Restore verification |
|---|---|---|

## Real-environment verification

## Not performed / unresolved

<!-- Never leave ambiguity: explicitly write what was not verified. -->
