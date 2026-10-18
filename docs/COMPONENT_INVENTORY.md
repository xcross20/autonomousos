# Component inventory

**Status:** source-verified catalog for human review. Nothing here is production-ready because a document says so. Nothing has been copied.

Labels: **VERIFIED** this session read the file at the named SHA. **REPORTED** an in-repo artifact describes an earlier run. **UNKNOWN** not inspected. Dispositions: ADOPT, ADAPT, REFERENCE, ARCHIVE, REJECT, UNKNOWN.

License for every code row: no LICENSE file. Do not copy until the owner allows it. Security: do not ingest secrets; the org-boot launcher and ERT redaction are denylists, not proof.

## Authority and queues

### VOS-GOV-PORTFOLIO — portfolio governor

- Source: `packages/control-plane/src/governor.ts`, `worker-loop.ts`. SHA `4b119d39d4ad71f7ff8a96a0ca7c22537f3fc1aa` on `feat/mission-control-v2`. Same paths exist on org-boot `b5a280a`; this session read the mission-control copies. Doc: `docs/PR16_BACKGROUND_AUTONOMY_PORTFOLIO_GOVERNOR.md`.
- Purpose: select and claim already-authorized, non-strategic work.
- Interface: `selectNext`, `MemoryWorkQueue.claim`, `runAutonomousTick`, `classifyFailure`, `BudgetLedger`. Statuses include queued, retry_ready, running, complete. Eligibility requires pause clear, active project, `strategic === false`, `authorityCeiling === green`, parallel caps, free scope lease, daily spend, attempt cap. Global cap `VENTUREOS_MAX_PARALLEL` default 4. Lease default 60s. Stale claims emit `job.recovered`.
- Persistence: tests use `MemoryStore` / in-memory queue. PR16 also names Postgres `background_jobs` helpers. Which store a live API process uses was not re-executed. **UNKNOWN** for a running server.
- Tests: `tests/acceptance/background-autonomy-portfolio-governor.test.ts` exists. **Not run this session.** PR16 lists 13 cases (score order, concurrency, scope conflict, idle, budget, retry, strategic refusal, pause, idempotent enqueue, soak).
- Defect: `runAutonomousTick` marks the job `complete` without a builder when `opts.runPipeline` is omitted and `payload.simulateFailure` is not a non-empty string. A non-empty `simulateFailure` throws instead. The tested tick is a queue simulator unless a host supplies `runGreenLoop`.
- Overlap: VOS-GOV-ORGBOOT.
- Disposition: **ADAPT** as the target authority. Do not call it the live product loop until a tick runs a real builder, test, and independent reviewer.

### VOS-GOV-ORGBOOT — supervised assignment governor

- Source: `scripts/org-boot-governor.ts`, `scripts/org-boot-resume.sh`, `docs/org-boot-003/queue.json`. Snapshot SHA `b5a280a916111777cc6a2e22184367788c59c99c`. That commit was ahead of origin `2d22aaf`. The worktree moved during review. Later observations are in `docs/REPOSITORY_LEDGER.md` and are not this component's pin.
- Purpose: the launcher that actually advanced Rally work orders.
- Interface: JSON queue version 1. Statuses `QUEUED | RUNNING | COMPLETED | BLOCKED`. `selectJob` returns the running approved non-strategic job, else the first queued approved non-strategic job. It refuses `main` and the wrong branch. Builder argv is `grok` with `--cwd`, `--prompt-file`, `--verbatim`, `--permission-mode dontAsk`, path `--allow`, `--max-turns 8`, `--no-subagents`, `--disable-web-search`, `--output-format json`. The throw if `--always-approve` or `bypassPermissions` is present only checks that constructed argv. Builder timeout 300s via `spawnSync`. Tests timeout 180s. Review prompt says do not use tools and replies ACCEPT or CHANGES_REQUIRED. Saves with temp file plus rename.
- Input that matters: each job carries `files[].contents`. The builder prompt orders those contents written exactly. This is supervised transcription plus test plus review, not open-ended planning.
- Persistence: one JSON file. Not MemoryStore. Not Postgres. Not an event log.
- Snapshot of the committed file at `b5a280a`: `maxAssignments` 18, 18 jobs, all `COMPLETED`, none `strategic`. 17 jobs are Rally. One is VentureOS, `org-boot-003-ventureos-limit`. Latest Rally source commits in that blob include `2142a2a`, `0c42251`, `174de84`, `3127d3a`, `e90f9d5`, `b0db0c1`, each with reviewer decision `ACCEPT`. A later working tree had more jobs, including queued sprint-006 work. That later file is not the snapshot.
- Tests: no unit test of the governor script was found in this read. Product tests were whatever each job's `test` command was. **Not re-run this session.**
- Defects: one process, no atomic claim across two workers, no descendant cleanup, no spend ledger, reviewer sees pasted assignment files rather than the full git diff, exact-file prompt can hide a rewrite outside the assertion if a test is narrow. Queue and product repo are two states that can diverge.
- Disposition: **REFERENCE** as the proven supervised fallback. **REJECT** as a second permanent state machine. Retire it only after the portfolio governor replays the same claim, test, review, and commit record.

### VOS-ROLE-GATES — action authority

- Source: `packages/governance/src/index.ts` at `4b119d3` (72 lines).
- Interface: `requiredAuthorityForAction`, `canExecute`, `canUseTool`, `builderCannotRelease`, `reviewerCannotMutate`.
- Tests: not run. File is small enough to read; behavior under every action string was not exhaustively executed.
- Disposition: **ADOPT** conceptually, **REFERENCE** until copy is licensed. These predicates should sit in front of any API mutation.

## Execution

### VOS-EXEC — worktree and verification helper

- Source: `packages/execution/src/index.ts` at `4b119d3`.
- Interface: `provisionSandbox`, `teardownSandbox`, `applyBuilderFiles`, `commitWorktree`, `revertWorktreeTo`, `runWorktreeVerification`, `diffWorktree`, `collectJourneyEvidence`.
- Dependencies: local git, filesystem. No pid-namespace killer was seen in the export list.
- Tests: not run this session.
- Overlap: ERT worktrees and org-boot `git commit` inside the governor.
- Disposition: **REFERENCE**. Useful sandbox helper. It is not a bounded process runtime.

### VOS-ORCH — package re-export

- Source: `packages/orchestrator/src/index.ts` re-exports `ControlPlane` and `createControlPlane` from the control plane. It is not a second implementation.
- Disposition: **ARCHIVE** as a name. Do not treat it as a second orchestrator.

### RALLY-GROK-PROVIDER — older autonomy launcher

- Source: `scripts/autonomy/grok_provider.py` in rally-org-boot at `210bbbc`. `launch_grok` builds `grok -p <prompt> --always-approve --cwd`.
- Tests: not run. A `live_block_reasons` symbol was not present in this tree.
- Disposition: **REJECT** for the organization launcher. `--always-approve` contradicts the org-boot governor. Keep the file in Rally history.

### ERT-PROCESS — bounded subprocess

- Source: `src/engineering_runtime/processes.py` at `21bac4c`. Last code change `ccc29b2`.
- Interface: `run_bounded(argv, cwd, env, timeout_sec) -> ProcessResult(exit_code, timed_out, stdout, stderr)`. New session, `ERT_RUN_ID`, inherited marker fd, process-group TERM then KILL, then kill by env marker, cwd, and marker-file holder. Output passes through `redact`.
- Tests: `tests/test_agent_adapter.py` covers several descendant shapes. Independent review of parent `2e349f6` **REPORTED** `109 passed` from the repository root. This session archived HEAD `21bac4c` to `/tmp/ert-verify` and ran `/tmp/ert-verify-venv/bin/python -m pytest -q` with cwd `/tmp/ert-verify` on Python 3.14.7: **109 passed in 50.12s, exit 0**. An earlier invocation from a different cwd failed 3 tests only because `SPEC = Path("infra/railway/controller.json")` is relative. That was an invocation error. The green run does not execute the R11-001 survivor and does not accept ERT-002.
- Note on VentureOS mission-control exports: committed `packages/mission-control/src/index.ts` at `4b119d3` stops at `executive.js`. The dirty worktree adds `operator-truth.js`, `repo-identity.js`, and `observe.js`. Those three are not part of the committed SHA.
- Defect **R11-001, unresolved:** `research/autonomous/cycles/ERT-002/limitations.md` states a descendant that closes inherited descriptors, clears the environment, and leaves the command directory can survive. `STATE.md` stage is `ERT-002_IMPLEMENTED_NOT_ACCEPTED`. Review r11 of `ef4fda7` returned `REPAIR_REQUIRED`. Later commits do not change `processes.py`.
- Disposition: **REFERENCE**, not ADOPT. Strongest process-lifetime code, blocked as the runtime.

### ERT-CYCLE — local-test controller

- Source: `src/engineering_runtime/cycle.py`, `controller.py`, `state_store.py`, `evidence.py`, `worktrees.py`, `agent_adapter.py` at `21bac4c`.
- Interface: `CycleRunner` over a SQLite store. `local-test` uses deterministic local agents, not paid Grok. Paid launch is refused because the CLI has no hard spend ceiling. Two-process lease test is named in the README (`test_cross_process_lease_has_one_owner`).
- Limits **VERIFIED** in `limitations.md`: SQLite owner id is the trust boundary; `controller.evaluate` is not the cycle path; recovery loses a candidate if the sandbox directory is deleted; redaction is a denylist.
- Disposition: **REFERENCE** for the evidence and lease shape. **REJECT** as a drop-in replacement while R11-001 is open and ERT-002 is not accepted.

### ERT-RAILWAY — execution backend stub

- Source: `src/engineering_runtime/execution_backend.py`, `infra/railway/controller.json` at `21bac4c`.
- Interface: `RailwaySandboxBackend.start` and `terminate` raise `ContainmentUnverified` before any subprocess or socket. `readiness_report` drops the environment argument.
- Evidence: `research/autonomous/cycles/ERT-002C-R/`. Decision `BLOCKED`. Containment `UNVERIFIED`, `trials_executed` 0. Reviewer of `2e349f6` recorded `NO_MATERIAL_DEFECTS` and said that is not acceptance. **REPORTED** pytest 109 passed on that parent.
- Disposition: **REJECT** for deployment. Railway is not an architectural prerequisite.

## Engineering loop roles

### VOS-EMERGENCE

- Source: `packages/emergence/src/index.ts` at `4b119d3`.
- Interface: `selectLenses`, `skepticGate`, `saturation`, `runEmergenceCycle`. `docs/AUTONOMY_MILESTONE.md` says bounded GREEN refinement can apply files via `EMERGENCE_IMPLEMENTER`, and that adaptive expert-performance weighting is still partial.
- Tests: not run this session.
- Disposition: **REFERENCE**. Proposals may enqueue only as non-strategic green work when the portfolio rule says so. Strategic proposals stay unclaimed.

### VOS-COUNCILS

- Source: `packages/councils/src/index.ts` at `4b119d3` (`composeCouncil`, `runCouncil`, disagreement pairs, foundry prompts).
- Milestone says council foundry output is used and cross-examination has three rounds. **REPORTED** in that doc. Not re-run.
- Disposition: **REFERENCE**. Not the default controller.

### VOS-SKILLS

- Source: `packages/skills/src/index.ts`. `classifyMutation`, `resolveAudits`, `aggregateReleaseReadiness` with pass/block/escalate. Skill files expected under `.agent/skills`.
- Disposition: **REFERENCE**. The organization bootstrap skill in this repo must call the governor, not replace it.

### VOS-EVIDENCE

- Source: `packages/evidence/src/index.ts`. `sha256`, `writeEvidence`. 33 lines. Not a full event log.
- Related: `packages/telemetry`, `packages/institutional-state`, `packages/db`. Present as directories. Internals not fully read. **UNKNOWN** beyond the path listing.
- Disposition: **ADAPT** later into one append-only event record. Do not invent a second log beside org-boot JSON and ERT SQLite until the kernel phase.

## Product strategy and operator surface

### VOS-RETHINK

- Source: `packages/mission-control/src/rethink.ts` at `4b119d3`.
- Interface: roles defender, falsifier, architect. `requiresHumanApproval` is true when a proposed change is mission, governance, or strategic. `applyRethink` / `rejectRethink` exist.
- Disposition: **ADOPT** as the strategic gate's rule. **REFERENCE** for the code. An agent may propose. It may not apply a strategic file without a human attribution.

### VOS-ASSIMILATE

- Source: `packages/repo-assimilator/src/index.ts`. `assimilatePath`, `discoverVerificationCommands`, readiness checklist.
- Disposition: **REFERENCE** for attach-existing-repo. Must stay read-only until an approval flag.

### VOS-CLI

- Source: `apps/cli/package.json` bin `venture` → `./src/index.ts`. Org-boot branch also has `apps/cli/src/org-boot-run-limit.ts` (from the queue history, commit `cc90e59` on that branch). Not re-read line by line this session. **UNKNOWN** how much of `venture org init` already exists. Do not claim the bootstrap CLI exists.
- Disposition: **ADAPT** after a command inventory. One CLI. AutonomousOS does not get a second binary in this phase.

### VOS-UI

- Source: `apps/web`, `apps/api`, mission-control pages. Committed SHA `4b119d3`. Working tree has 29 uncommitted edits (layout, mission-control page, routes, store).
- Disposition: **UNKNOWN** for the dirty tree. **REFERENCE** for the committed shell as the operator projection. UI must not mutate job state except through the API. Do not deploy.

## Rally product

### RALLY-WORKBENCH

- Source: `feat/org-boot-001-rally` `210bbbc`. Domain scanner, DexScreener observations, Nansen separation, contract watchlist. Files live in that repo (`src/workbench/`, dashboard). This audit did not re-execute the browser checks.
- At snapshot `210bbbc`, root has `MISSION.md`, `STRATEGY.md`, `NEXT.md`, `STATE.md`, and no `RETHINK.md`. `engineering/` at that SHA has `MISSION.md`, `NEXT.md`, and `STATE.md` only. It does not have `STRATEGY.md`.
- Disposition: **REFERENCE**. Domain code stays in Rally. The accepted loop is a fixture for the organization, not a module to import. Do not interrupt the live checkout.

### RALLY-RESEARCH

- `scripts/research`, `scripts/strategy_lab`, `scripts/sell_lab`, `scripts/execution_lab`, `scripts/portfolio_lab`.
- Disposition: **REFERENCE**. Research scripts are product science. They are not the engineering governor. `execution_lab` is a trading scenario engine name, not ERT.

## Council Lab

### CL-STATE

- Source: `src/council_lab/state.py` at `dec2aae`.
- Transitions: `PROPOSED → READY | BLOCKED`, `READY → RUNNING | BLOCKED`, `RUNNING → REVIEW | BLOCKED | SAFETY_STOP`, `REVIEW → ACCEPTED_LOCAL | READY | BLOCKED`, `ACCEPTED_LOCAL → INTEGRATED | BLOCKED`, `INTEGRATED → VERIFIED | BLOCKED`, `BLOCKED → READY`. `VERIFIED` and `SAFETY_STOP` have no outbound edge. SQLite mode `0600` on create. Comment: no LLM has write authority.
- Disposition: **REFERENCE**. Different state names from VentureOS and org-boot. Do not merge the enums. Map at the boundary.

### CL-HARNESS

- Modules: `cycle.py`, `runner.py`, `recovery.py`, `reviewer.py`, `builder.py`, `verifier.py`, `trust.py`, `redact.py`, `evaluation.py`, `scenarios.py`, `replay.py`, `cli.py`.
- README (**VERIFIED**): CLI can init, mission, status, task, transition, one council cycle, resume, and `run --max-cycles`. Autonomous councils and publication benchmarks are not implemented. V3 prompts are stored and not enabled. No live provider calls in the foundation.
- `docs/NEXT.md`: CL-REL-002 is `READY_FOR_INDEPENDENT_QA`, which the file says is not acceptance. Oracle protection commits are on this history (`568a550` protect oracle directory, `33089ac` reject submission subclasses). This session did not re-prove the oracle boundary.
- CL-REL-003A at `dec2aae` / `b93fd45` is a development comparison runner. Local only.
- Disposition: **REFERENCE** as the research-integrity adapter. **UNKNOWN** as a passed blinded benchmark. Do not let a council conclusion enqueue strategic work.

## File conventions that collide

| File | Where it is authoritative | Where it is only a projection |
| --- | --- | --- |
| Queue | Portfolio governor store, once selected | Org-boot `queue.json` until cutover |
| `STATE.md` | Nowhere as a queue | Rally, Council Lab, ERT each have one |
| `NEXT.md` | Authorized work only after a decision | Each repo has its own |
| `STRATEGY.md` | Product repo, human-approved | Agents may propose via RETHINK |
| `RETHINK.md` | Council Lab has a root file. Rally find did not show one. VentureOS uses `rethink.ts` records | Do not create three copies |

## Critical missing capabilities

1. One command from a mission sentence to a reviewed vertical slice.
2. An accepted process runtime that reaps the R11-001 descendant.
3. A two-worker atomic claim on the queue that has real product evidence.
4. A reviewer that inspects the full diff, not only the files named in the prompt.
5. Recorded token cost and wall-clock per accepted job.
6. A license to extract code.
7. Enforced monetary ceiling on the Grok CLI (ERT already refuses paid launch for this reason).
8. Adaptive expert weighting (milestone: partial).
9. Pushed copies of the Council Lab comparison branch and the ERT-002c branch. They exist only locally.
