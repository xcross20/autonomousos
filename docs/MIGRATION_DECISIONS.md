# Migration decisions

Founder sheet for the blueprint risks. Recommendations are proposals. Nothing here starts Phase B.

## R1 — Canonical base

Recommendation: upgraded VentureOS, not a new runtime inside AutonomousOS. ADR-001.

Evidence: VentureOS already has the governor, API, CLI, UI, councils, emergence, evidence helper, and assimilator. AutonomousOS at the start of this work had no packages. A second repo would duplicate that code.

Decision needed: confirm or reject ADR-001. Rejection still requires a license and a SHA-pinned extraction. It does not authorize a bulk copy.

## R2 — Runtime

Recommendation: keep org-boot `spawnSync` (`dontAsk`, 300s) as the host executor. Do not switch to ERT `run_bounded`. Do not enable `packages/deploy` live mode. Do not enable ERT Railway.

Evidence: R11-001 remains in `limitations.md`. ERT stage is `ERT-002_IMPLEMENTED_NOT_ACCEPTED`. Containment trials are 0. `createDeploy` throws if live mode lacks `RAILWAY_API_TOKEN`. This pass did not create that token and did not deploy.

Decision needed: accept the host executor until a new ERT commit kills the detached descendant and an independent review accepts it.

## R3 — Branch divergence

Recommendation: adoption pins stay at the snapshot SHAs. Live org-boot HEADs are observations only.

Evidence: during the first review the queue moved. At the start of this pass (`2026-10-18T00:31:51Z`) VentureOS org-boot was `60704c87ea56d7987a6c4bfe05a178a4df59ee0f` (ahead 15) and Rally org-boot was `aa304f15d1cf1e21d77e72dd418b8bf5e215d369` (ahead 30). Those are not the adoption pins. The adoption pins remain `b5a280a` and `210bbbc` for the trees that were read in detail.

Decision needed: none, except do not migrate from a moving worktree.

## R4 — Research integrity

Recommendation: Council Lab stays a reference adapter. Do not treat `READY_FOR_INDEPENDENT_QA` as acceptance. Do not promote a council result into a trading or product claim.

Evidence: `docs/NEXT.md` at `dec2aae` says that status is not acceptance. The branch is not on the remote. Oracle-boundary tests were not re-proven by a blinded protocol in this pass.

Decision needed: authorize Council Lab QA separately before any research adapter is connected.

## R5 — Strategic overreach

Recommendation: `requiresHumanApproval` stays the rule. A rethink proposal is not `venture approvals approve` of a strategy change unless the stored change kind is mission, governance, or strategy and the actor is a human.

Evidence: the function returns true for those kinds. No test of that function was run in this pass. The portfolio governor already refuses `strategic === true` for automatic claim. That refusal is part of the 17 passing governor tests (the file covers the PR16 cases, including strategic refusal, as named in `docs/PR16_BACKGROUND_AUTONOMY_PORTFOLIO_GOVERNOR.md`; this pass did not re-print each case name).

Decision needed: confirm that strategic apply is a human event. The code already says so. The missing piece is a test that was not run here.

## R6 — Complexity

Recommendation: the first implementation order, after written acceptance of Gates A–C, is only the read-only mapping of the `b5a280a` queue blob onto `GovernorJob`. No councils, no UI rewrite, no ERT, no new service.

Evidence: the blueprint’s lean default is one governor, one accepted runtime, two workers only after a pilot, councils on demand. Two workers are not proven. The org-boot selector returns one job.

Decision needed: authorize that mapping test, or stop.

## Explicit non-decisions

Phase B schema, bootstrap CLI changes, Gate D, screenshots, and a cost report with dollars are not decided because the measurements do not exist. Unknown is not a pass.
