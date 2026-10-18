# Architecture decisions

## ADR-000 — Inventory before migration

Status: accepted for this phase.

Code stays in the source repositories. This branch records evidence, ownership, and the next gate. A diagram is not a running organization.

## ADR-001 — VentureOS is the canonical code host

Status: proposed for founder confirmation.

Decision: keep executable governance, persistence, API, CLI, and UI in VentureOS. Keep AutonomousOS as the integration ledger, skill contract, and review packet. Do not promote AutonomousOS into a second monorepo in this phase.

Why:

- VentureOS already contains the control-plane governor, governance predicates, execution helpers, emergence, councils, evidence helper, assimilator, mission-control rethink, and the `venture` CLI.
- The org-boot line is a VentureOS script and a JSON file, not a new product. At snapshot `b5a280a` that file held 18 completed jobs: 17 Rally and 1 VentureOS. The worktree continued after the snapshot.
- Engineering Runtime is a separate repository with an unaccepted runtime and an unresolved descendant defect. Folding it in by rename would hide that defect.
- Council Lab and Rally own research protocols and product code. The blueprint says they remain separate.
- AutonomousOS at `2f61e00` had no packages. Starting over there repeats V1–V4.

Consequences: future kernel work lands in VentureOS behind an approval, or stays unwritten. AutonomousOS PRs are documents until the owner reverses this ADR.

Rollback: if the founder wants a clean repo anyway, the extraction still starts from the SHAs in `docs/SOURCE_MANIFEST.yaml`, with tests, and only after a license grant. This ADR does not forbid that later. It forbids doing it silently.

## ADR-002 — One job authority

Status: proposed.

The portfolio governor (`selectNext`, claim, scope lease, budget, pause, `strategic === false`) is the target authority. `scripts/org-boot-governor.ts` remains the fallback launcher and is not extended. Both must not be active on the same job.

Strategic work never becomes runnable because a research or rethink agent proposed it. Human approval is a separate attributed event. `requiresHumanApproval` in `rethink.ts` is the current predicate for mission, governance, and strategy files.

IDLE is a successful outcome when no eligible job exists.

## ADR-003 — Do not adopt Engineering Runtime yet

Status: accepted as a blocker.

R11-001 remains open: a descendant that closes inherited descriptors, drops `ERT_RUN_ID`, and leaves the command directory can survive `run_bounded`. `processes.py` has not changed since `ccc29b2`. ERT-002 stage is `ERT-002_IMPLEMENTED_NOT_ACCEPTED`. ERT-002C containment is `UNVERIFIED` with zero trials. Railway start and terminate raise before any call.

The org-boot `spawnSync` timeout is the supervised host executor. It also does not reap escaped descendants. That is a known gap, not a reason to switch to a blocked runtime.

No cloud adapter and no Railway deploy in this phase.

## ADR-004 — Loops share one state machine

Status: proposed.

Engineering, product-improvement, strategy, research, and workflow experiments are job types and gates on the same queue. They do not get private controllers.

- Engineering: plan, critique, build, test, independent review, decision.
- Product improvement: emergence may propose. Only bounded non-strategic green work can be queued for automatic claim.
- Strategy: rethink proposal, adversarial critique, human decision, then an authorized job.
- Research: Council Lab protocol returns a conclusion, including a negative one. It does not enqueue product work.
- Self-improvement: a workflow change is an experiment with a baseline. An LLM preference is not adoption.

Council Lab's transition names stay inside Council Lab. A mapping table is allowed. A merged enum is not.

## ADR-005 — Projections are not queues

Status: proposed.

`STATE.md` and dashboards are rendered from durable records. `NEXT.md` changes only after an authorization event. Product repositories may keep their current files. This phase does not rewrite them.

## ADR-006 — Parallelism starts at two workers and is not implied by extra sessions

Status: proposed.

The org-boot selector is single-threaded. The portfolio governor has a parallel cap and a scope lease, tested in memory, not against two live builders. The first concurrency proof is two jobs, disjoint paths, one interrupted worker, one integration review. Until that passes, the cap for real builders is one.

## ADR-007 — Builder cannot accept its own work

Status: accepted as policy already present in source.

`builderCannotRelease` and the org-boot split between builder session and reviewer session stay in force. The current org-boot reviewer does not see the full diff. The next implementation gate must close that gap before anyone calls review independent in the strong sense.

## ADR-008 — License and secrets block extraction

Status: accepted.

No LICENSE file was found. No secrets are copied into manifests. External repository text is untrusted. Paid model calls and Railway resource creation stay unauthorized.
