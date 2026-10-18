# Integration plan

Ordered so each step can be rejected without a bulk import. Gates A–C are this document set. Gate D does not start until a human accepts A–C and writes an implementation authorization. This plan does not perform Gate D.

## Gate A — Inventory

Done as a proposal in `docs/REPOSITORY_LEDGER.md`, `docs/COMPONENT_INVENTORY.md`, and `docs/SOURCE_MANIFEST.yaml`.

Exit criteria:

- Every named source has a path, branch, and SHA, or an explicit UNKNOWN.
- ERT is identified as `xcross20/engineeringruntime`. No FDE repo was found.
- R11-001 is still open.
- Dirty and unpushed work is listed and was not edited.

## Gate B — Architecture

Proposal in `docs/ARCHITECTURE.md` and `docs/DECISIONS.md`.

Exit criteria:

- One owner for job authority: VentureOS portfolio governor, with org-boot as fallback only.
- One owner for product code: the product repo.
- One owner for research-integrity experiments: Council Lab.
- Execution owner: none of the candidates is accepted as a replacement. Fallback remains org-boot `spawnSync`.
- Canonical code host: VentureOS. AutonomousOS remains the ledger.

## Gate C — Migration readiness

Not passed. A later change may pass a single row only when all of these exist:

- Source URL, branch, and full SHA.
- A test that failed for the missing behavior and then passed for the real behavior, run outside the dirty product trees.
- A compatibility note against the org-boot queue records.
- A rollback that leaves `scripts/org-boot-resume.sh` usable.
- A license grant if bytes move.
- An independent reviewer who did not write the change.

No row in the matrix meets that bar today. REFERENCE rows do not need a copy.

## Sequence after authorization

Do these one at a time. Stop on the first failed gate.

1. **Map, do not move.** Write a read-only projection from one org-boot `Job` onto the portfolio `GovernorJob` fields. Contract test: strategic jobs are not selected; a second claim of the same id fails; replay of the snapshot blob at `b5a280a` (18 completed records) does not enqueue them again. Re-read the live file first. It will not still be that blob.
2. **Review gap.** Change the fallback review prompt so the reviewer receives `git diff` for the allowed paths. Test that a byte change outside the asserted lines is visible. Do this in a copy or a new VentureOS branch, never on the live Rally sprint branch.
3. **Real tick behind a flag.** `runAutonomousTick` must not mark complete when `runPipeline` is absent, except in the existing simulator tests. The flag default stays off.
4. **Keep one launcher.** New jobs enter only the portfolio queue. The JSON governor is frozen as the resume path for already completed history.
5. **Evidence event.** One append-only record per transition, with actor and artifact hash. `STATE.md` rendered from it.
6. **Bootstrap command** as specified in `docs/BOOTSTRAP_CONTRACT.md`, still calling VentureOS. Idempotent second run.
7. **Emergence and rethink** only as proposal events. Human attribution before strategic apply.
8. **Council Lab adapter** after its own QA accepts the oracle boundary. Conclusions do not enqueue.
9. **UI** on a committed SHA only. No use of the dirty mission-control tree.
10. **ERT adapter** only after a new independent review shows the R11-001 descendant is dead, plus recovery and redaction. Until then the adapter is not compiled into the default path.
11. **Two-worker pilot** from `docs/VALIDATION_PLAN.md`. Cap stays at one until that pilot passes.

## Explicit non-actions

- No merge to any `main`.
- No deploy, including Railway.
- No edit to Rally live work, the org-boot queue, or ERT sources from this repository.
- No bulk copy of prompts, generated artifacts, `.env` files, or historical UI experiments.
- No paid model calls.

## Compatibility

Org-boot statuses map forward only as a projection:

| Org-boot | Portfolio target | Notes |
| --- | --- | --- |
| QUEUED + approved + not strategic | queued, authority green | |
| QUEUED + strategic | blocked from auto-claim | org-boot already skips these |
| RUNNING | claimed or running | single owner |
| COMPLETED | complete | do not re-run |
| BLOCKED | blocked | no silent retry |

Council Lab names (`ACCEPTED_LOCAL`, `SAFETY_STOP`, `VERIFIED`) stay on that side of the adapter.

## Rollback

Each step ships behind the previous launcher. If a new tick mis-claims, stop the process, set the governor pause, and resume with `scripts/org-boot-resume.sh` only for jobs that were never copied into the new queue. Completed JSON records are not rewritten.

## Complexity

The minimum kernel (steps 1–6) is a small, safety-sensitive change set in VentureOS: a mapping, a stricter tick, a diff-based review, and one CLI entry. It is not a new platform.

Unifying every V1–V4 package, both council systems, and ERT is a large program and is not required for the first accepted slice. ERT adoption is blocked, so its cost is unbounded until R11-001 has a failing test that goes green.

Estimated cost of this discovery phase: no paid model calls. Wall time is one audit session. Token totals were not metered by the tools available here.
