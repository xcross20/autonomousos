# QA report — ORG-CONSOLIDATE-001

Independent reviewer session: `01a14c5d-2216-7802-824f-d8b989f62665`.

The reviewer did not write the inventory. The author did not treat this review as acceptance. Gate D does not start.

## Verdict

**CHANGES_REQUIRED**

The reviewer refused Gate D. After the review, the author corrected the false pins named below. Those corrections were not independently re-reviewed. The packet is ready for a human to read with that limit stated. It is not an accepted architecture and it is not a built organization.

## What the reviewer checked and held

- `selectJob`, branch refusal, temp-file save, exact-file builder prompt, `dontAsk` argv, 300s and 180s timeouts, review prompt without tools.
- `runAutonomousTick` at `4b119d3` completes with no builder when `runPipeline` is omitted and `simulateFailure` is empty.
- `processes.py` last commit `ccc29b2159869cdd956dc41243d6e8086a45a0d0`. ERT-002 not accepted. R11-001 still described as a surviving descendant. Railway start and terminate raise `ContainmentUnverified`. Destroy trials 0.
- `launch_grok` uses `--always-approve`.
- No LICENSE file at the four roots. VentureOS `package.json` is private.
- Canonical host text agrees: VentureOS for code, AutonomousOS for the audit, no deploy.
- The packet does not claim Gate D ran and does not claim the organization exists.

## Material defects the reviewer found

1. The ledger called VOS-BOOT HEAD `b5a280a` and clean. During the review the live governor had moved that worktree (reviewer read `f4978f1`, queue in progress).
2. "18 Rally jobs" was wrong at `b5a280a`: 17 Rally and 1 VentureOS.
3. Rally-boot HEAD `210bbbc` was also stale by the review (`d89fd6b`).
4. `engineering/STRATEGY.md` is not in `210bbbc:engineering`. Root `STRATEGY.md` is.
5. This file did not exist when the README pointed at it. The pytest sentence had no in-repo log.

## Author corrections after the review

These sentences were changed in the ledger, inventory, decisions, efficiency baseline, and source manifest. They are not a new review.

- `b5a280a` and `210bbbc` are labeled as the audit snapshot, not as the live HEAD.
- Drift table records the reviewer's read and one later author read: committed VentureOS org-boot `826f1226644b52e2607240c20d0a744533722d96` with a dirty queue, and Rally org-boot `0ec92b387f77b2b2fedc01d6239ee1008f8395c1` with a dirty workbench file. Both can already be older than the worktree.
- Job mix at the snapshot is 17 Rally and 1 VentureOS.
- `engineering/STRATEGY.md` claim removed.
- Portfolio-governor wording includes the `simulateFailure` throw.
- Council Lab `BLOCKED → READY` is included.
- Orchestrator re-exports `createControlPlane` as well.

Pytest that this session did run, from the command log: `git archive HEAD` of engineeringruntime while HEAD was `21bac4c9ba24db6b47c433440ddb1bf071ca634f`, extracted to `/tmp/ert-verify`, then `/tmp/ert-verify-venv/bin/python -m pytest -q` with cwd `/tmp/ert-verify`. Result: **109 passed in 50.12s, exit 0**, Python 3.14.7. An earlier run from a different cwd failed 3 tests because `infra/railway/controller.json` is a relative path. The reviewer did not re-run pytest and did not accept that log as tied to the commit. The green run does not execute the R11-001 survivor and does not accept ERT-002.

## Duplicate ownership

Still unresolved in code. The portfolio governor is the proposed owner. The JSON governor was running during the review. Both mechanisms still exist. Gate C is not passed.

## Gate D

Do not start. Human acceptance of Gates A–C is not in this packet. The live queues are a moving source. Implementation is not authorized.

## Tests this review did not run

The first reviewer did not run VentureOS vitest, Council Lab unittest, Rally product tests, or the org-boot governor. The reviewer did not re-run pytest.

## Blueprint-gap pass, before the second review

Isolated from the live trees:

- Archive of VentureOS `4b119d3`, `npx vitest run tests/acceptance/background-autonomy-portfolio-governor.test.ts`: **17 passed**, vitest 3.2.7, exit 0. `node_modules` was a symlink to the existing checkout. Source status stayed dirty only on the files that were already dirty.
- Archive of Council Lab `dec2aae`, venv install, `unittest discover`: **116 tests, OK, 9.370s**. Not an oracle-boundary acceptance.
- Live HEADs at `2026-10-18T00:31:51Z` and `2026-10-18T00:39:21Z` were the same: org-boot `60704c8`, Rally `aa304f1`.

New pages: `docs/CAPABILITY_MATRIX.md`, `docs/DUPLICATION_REGISTER.md`, `docs/MIGRATION_DECISIONS.md`, `docs/OPERATOR_RUNBOOK.md`, `docs/BRANCH_MATRIX.md`, `docs/PROMPT_INVENTORY.md`, `docs/BLUEPRINT_COVERAGE.md`. Root `SOURCE_MANIFEST.yaml` only points at `docs/SOURCE_MANIFEST.yaml`.

Gate D is still not started. `venture org init` is still not a real command.

## Second review

Reviewer session `01a14c73-74fd-7751-9ffc-21d97161e475` returned **CHANGES_REQUIRED**. Gate D must not start.

The reviewer checked the discovery checklist rows, the role-prompt map, both `010_` migrations, `recoverStale`, and the live HEADs at `2026-10-18T00:40:41Z` (`60704c8` and `aa304f1`, unchanged). Those checks held.

One false sentence: the bootstrap contract said `venture work start` was absent from `apps/cli/src/index.ts`. The help text omits it. The handler at line 155 is `work` + `start` and POSTs `/api/work-cells/${id}/start`. It is not `venture work start --mission`. The contract and the runbook were corrected from that file after the review. The correction was not re-reviewed.

## Decision

Documentation packet is **READY_FOR_HUMAN_REVIEW** with that one correction unreviewed. The organization is not built. Gate D is not authorized. Founder decisions R1–R6 stay open.
