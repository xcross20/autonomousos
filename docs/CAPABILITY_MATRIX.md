# Capability matrix

Checklist from the consolidation blueprint. Snapshot for VentureOS code is committed `4b119d39d4ad71f7ff8a96a0ca7c22537f3fc1aa` on `feat/mission-control-v2`, unless a row names another SHA. The dirty worktree is not a source. Confidence: **VERIFIED** this pass opened the file or a `git grep`/`git show` of that SHA. **REPORTED** an in-repo document describes a run this pass did not repeat. **PROPOSED** target only. **UNKNOWN** not opened.

Disposition does not authorize a copy. No LICENSE file was found.

## Code

| Item | Source | SHA | Confidence | Owner | Disposition |
| --- | --- | --- | --- | --- | --- |
| Portfolio governor | `packages/control-plane/src/governor.ts`, `worker-loop.ts` | `4b119d3` | VERIFIED | VentureOS | ADAPT. Target authority. `recoverStale` returns expired `claimed`/`running` jobs to `retry_ready` |
| Org-boot governor | `scripts/org-boot-governor.ts` | snapshot `b5a280a`; live tree moved | VERIFIED at snapshot | VentureOS script, fallback | REFERENCE. One worker. Exact file bodies in the queue |
| Controllers | ERT `src/engineering_runtime/controller.py`, `cycle.py`; Council Lab `src/council_lab/cycle.py` | ERT `21bac4c`; Council `dec2aae` | VERIFIED paths | Each repo | REFERENCE. Not the VentureOS governor |
| Queue | `MemoryWorkQueue`; org-boot `queue.json`; Council SQLite | as above | VERIFIED | One target: portfolio queue | REFERENCE until cutover. Do not run both |
| Lease and recovery | `claim` / `recoverStale` in `governor.ts`; ERT lease tests; Council `recovery.py` | `4b119d3` for the method body | VERIFIED method. ERT tests REPORTED by prior 109-pass run | VentureOS for product jobs | ADAPT later. Org-boot has no cross-process lease |
| Runtime adapters | `packages/agents/src/index.ts` (`RuntimeAdapter`, provider classes, `invokeRole`); `packages/agents/src/executor.ts` (`ManualSessionExecutor`, `executorFor`) | `4b119d3` | VERIFIED exports | VentureOS | REFERENCE. Paid adapters exist. This pass did not call them |
| Terminal / PTY | `packages/terminal/src/index.ts` `openTerminal`, `runCommand`, `PtyHandle` | `4b119d3` | VERIFIED exports | VentureOS | REFERENCE. Not the job runner |
| Subprocess cleanup | ERT `processes.py` | code `ccc29b2` on `21bac4c` | VERIFIED limitation text | None until R11-001 is fixed | REFERENCE |
| Worktrees | `packages/execution` sandbox helpers; `apps/api/lib/git/worktree.ts`; ERT `worktrees.py` | `4b119d3` paths; ERT `21bac4c` | VERIFIED paths. Execution behavior not re-run | Product repo worktree plus VentureOS helper | REFERENCE |
| Role dispatch | `invokeRole` and `PromptRegistry` role map | `4b119d3` | VERIFIED | VentureOS | REFERENCE |
| Reviewers | Org-boot review prompt; ERT reviewer subprocess; `prompts/v1/SYSTEM_REVIEWER.md`; governance `reviewerCannotMutate` | mixed | VERIFIED files. Org-boot review does not receive `git diff` | Separate session from the builder | ADAPT the review input later |
| Auditors | `.agent/skills/*/SKILL.md`; `packages/skills` `resolveAudits` | `4b119d3` | VERIFIED skill file names and export | VentureOS | REFERENCE. Fail-closed behavior not re-run |
| Release judge | `prompts/v1/RELEASE_JUDGE.md`; `aggregateReleaseReadiness`; `packages/github/src/merge-gate.ts` `evaluateMergeGate` | `4b119d3` | VERIFIED | VentureOS | REFERENCE. Merge stays human |
| GitHub integration | `packages/github` `createGitHub`, modes `test` / `simulation` / `live`; routes under `/api/github/` | `4b119d3` | VERIFIED | VentureOS | REFERENCE. Live mode needs a token this pass did not use |
| Deployment adapters | `packages/deploy` `createDeploy`; `RailwayDeploy` only when mode is live and configured; fixture otherwise. ERT backend raises `ContainmentUnverified` | `4b119d3` and ERT `21bac4c` | VERIFIED | None for this phase | REJECT as a step in the kernel. `RAILWAY_API_TOKEN` missing throws in live mode |

Portfolio governor test, this pass: archive of `4b119d3` at `/tmp/vos-gov-test`, `npx vitest run tests/acceptance/background-autonomy-portfolio-governor.test.ts`, vitest 3.2.7. **17 passed**, one file, exit 0. Dependencies were the existing VentureOS `node_modules` via a symlink. Source files in `/Users/immanuellewis/ventureos` were not edited. This is not a Postgres or live-API run.

## Intelligence

| Item | Source | SHA | Confidence | Owner | Disposition |
| --- | --- | --- | --- | --- | --- |
| Emergence governor | `prompts/v3/emergence/EMERGENCE_GOVERNOR.md`; `runEmergenceCycle` | `4b119d3` | VERIFIED call and path | VentureOS, on demand | REFERENCE |
| Reference curator | `prompts/v3/emergence/REFERENCE_CLASS_CURATOR.md`; `runEmergenceCycle` loads it | `4b119d3` | VERIFIED | VentureOS | REFERENCE |
| Skeptic | `skepticGate`; `EMERGENCE_SKEPTIC.md`; council archetype `adversarial_skeptic` | `4b119d3` | VERIFIED | VentureOS | REFERENCE |
| Saturation | `saturation()`; `EMERGENCE_SATURATION_JUDGE.md` | `4b119d3` | VERIFIED | VentureOS | REFERENCE |
| Rethink evaluator | `packages/mission-control/src/rethink.ts` | `4b119d3` | VERIFIED | VentureOS | REFERENCE. Human gate for mission, governance, strategy |
| Expert councils | `packages/councils` `runCouncil`; Council Lab CLI | `4b119d3` / `dec2aae` | VERIFIED | Council Lab for research integrity. VentureOS package for product deliberation | REFERENCE. Not the default controller |
| Research protocol designer | `prompts/v3` council `COUNCIL_EXPERIMENT_DESIGNER.md`; Council Lab `evaluation.py`, `scenarios.py` | `4b119d3` / `dec2aae` | VERIFIED paths | Council Lab | REFERENCE |
| Causal evidence and outcome evaluator | `packages/institutional-state` beliefs (`known` / `believed` / `unknown` / `falsified`) | `4b119d3` | VERIFIED class. No separate causal-outcome module was found | VentureOS for beliefs | UNKNOWN as a causal estimator. Do not invent one |
| Self-improvement scoring | `docs/AUTONOMY_MILESTONE.md` lists adaptive expert weighting as still partial. Prompt `COUNCIL_PERFORMANCE_EVALUATOR.md` exists. No weighting function was found in `packages/councils/src/index.ts` | `4b119d3` | VERIFIED absence in that file. REPORTED partial status | None | UNKNOWN implementation. Do not adopt a score |

## Product and interface

| Item | Source | SHA | Confidence | Owner | Disposition |
| --- | --- | --- | --- | --- | --- |
| Repo assimilator | `packages/repo-assimilator`; CLI `venture project assimilate` | `4b119d3` | VERIFIED | VentureOS | REFERENCE. Read-only until an approval flag |
| Mission onboarding | Web `OnboardingPage.tsx`, `CreateVenturePage.tsx`; CLI `venture project add` | `4b119d3` | VERIFIED paths. Screen behavior not browsed | VentureOS | REFERENCE |
| Web shell | `apps/web/src/features/**` pages listed in the branch matrix note | `4b119d3` committed paths | VERIFIED names. Dirty tree has extra edits and was not used | VentureOS | UNKNOWN whether each page is wired. Do not deploy |
| API routes | `apps/api/src/app.ts` exact paths below | `4b119d3` | VERIFIED by `git grep` | VentureOS | REFERENCE. Dynamic `/:slug` routes were not fully listed |
| CLI | `apps/cli/src/index.ts` | `4b119d3` | VERIFIED | VentureOS | ADAPT later. See runbook. `venture org init` is absent. `venture work start <id>` posts `/api/work-cells/<id>/start` and is not the proposed `--mission` command |
| Project templates | `templates/product-repository/` in AutonomousOS | this branch | PROPOSED | AutonomousOS docs | REFERENCE. Not applied to any product repo |
| Design system | `.agent/skills/design-qa/SKILL.md`; web feature CSS on the committed tree | `4b119d3` | VERIFIED skill path. Full token set not audited | VentureOS | UNKNOWN as a single system. Do not import PIX from elsewhere in this phase |
| UX evaluator | `prompts/v3/emergence/UX_RESEARCHER.md`, `VISUAL_DESIGN_DIRECTOR.md` | `4b119d3` | VERIFIED paths | VentureOS, on demand | REFERENCE |
| Browser tests | `tests/playwright/` | `4b119d3` | VERIFIED directory exists. Not run | VentureOS | UNKNOWN result |
| Customer journey contracts | `collectJourneyEvidence` in `packages/execution`; `runNamedJourneys` in `packages/deploy` | `4b119d3` | VERIFIED exports. Not executed | VentureOS | REFERENCE |

Committed API paths seen with `pathname ===` in `apps/api/src/app.ts` at `4b119d3`: `/api/chatkit`, `/api/doctor`, `/api/runtime`, `/api/governor`, `/api/governor/tick`, `/api/governor/enqueue`, `/api/governor/pause`, `/api/governor/jobs`, `/api/fixtures/operator-shell`, `/api/admin/origin-stats`, `/api/admin/cleanup-ventures`, `/api/snapshot`, `/api/projects`, `/api/ventures`, `/api/work-cells`, `/api/agent-profiles`, `/api/councils`, `/api/emergence`, `/api/mcp`, `/api/mcp/authorize`, `/api/mcp/bindings`, `/api/mcp/bind`, `/api/secrets/status`, `/api/deploy/status`, `/api/deploy/doctor`, `/api/providers`, `/api/models`, `/api/models/select`, `/api/profiles`, `/api/approvals`, `/api/events`, `/api/evidence`, `/api/leases`, `/api/github/repos`, `/api/github/repos/create`, `/api/github/doctor`. Mission control prefix `/api/mission-control` plus `scan`, `attach`, and `missions` in `mission-control-routes.ts`. The dirty copy of that file is not this list.

## State and policy

| Item | Source | SHA | Confidence | Owner | Disposition |
| --- | --- | --- | --- | --- | --- |
| Schemas | `packages/db/src/schema.sql` | `4b119d3` | VERIFIED path | VentureOS | REFERENCE |
| Migrations | `packages/db/src/migrations/001` through `011`, two files numbered `010` | `4b119d3` | VERIFIED names. SQL not executed | VentureOS | REFERENCE. Two `010_*.sql` files are a drift risk. UNKNOWN which order a fresh database applies |
| Event logs | `GET /api/events`; org-boot JSON is a snapshot rewrite; ERT SQLite | `4b119d3` | VERIFIED route and the two other stores | Not unified | ADAPT later into one append-only log |
| Budget and authority | `BudgetLedger`, `maxGlobalParallel` default 4, `packages/governance` | `4b119d3` | VERIFIED | VentureOS | REFERENCE. Org-boot has no spend field |
| Skill registry | `packages/skills` `SKILL_IDS`; `.agent/skills/*/SKILL.md` (19 files) | `4b119d3` | VERIFIED names | VentureOS | REFERENCE |
| Prompt versions | `prompts/v1` (11 files), `prompts/v3` (60 files), registry in `packages/prompts/src/index.ts` | `4b119d3` | VERIFIED counts and role map | VentureOS | REFERENCE. Not each file was opened |
| Agent profiles | `GET/POST /api/profiles`, `/api/agent-profiles`; CLI `venture agent profile` | `4b119d3` | VERIFIED routes and help text. Profile schema body not re-read | VentureOS | REFERENCE |
| Provenance | `packages/evidence` `sha256`; institutional beliefs `evidenceRefs` | `4b119d3` | VERIFIED small helper | VentureOS | ADAPT later |
| Evidence hashes | `writeEvidence`, `sha256` | `4b119d3` | VERIFIED 33-line package | VentureOS | REFERENCE |
| Incident handling | `prompts/v1/INCIDENT_DIAGNOSTICIAN.md` mapped in `ROLE_FILES` | `4b119d3` | VERIFIED path. No incident state machine was opened | VentureOS prompt | UNKNOWN as an executable loop |

## Documentation

| Item | Source | Confidence | Disposition |
| --- | --- | --- | --- |
| V1–V4 | `docs/03_V1_CONTROL_PLANE`, `docs/04_V2_EMERGENCE`, `docs/05_V3_COUNCILS`, `docs/06_V4_OPERATOR_PRODUCT`, plus `docs/02_CANONICAL_ARCHITECTURE_AND_PRODUCT_MODEL.md` | VERIFIED directories exist at `4b119d3`. Bodies not copied | REFERENCE. Packages above are the code. The docs are not a second runtime |
| NEXT / STATE / STRATEGY / RETHINK | Ledger and inventory. Rally `engineering/` at `210bbbc` has no `STRATEGY.md` | VERIFIED for that SHA | Projections. Not queues |
| Operational runbooks | `docs/OPERATOR_RUNBOOK.md` in this repo | PROPOSED for the fallback path. Source repos have their own NEXT files | REFERENCE |
| QA evidence | This file’s vitest line; prior ERT 109; `QA_REPORT.md` | VERIFIED for the commands named there | Not Gate D |
| ADRs | `docs/DECISIONS.md` | VERIFIED | Proposed until the founder confirms ADR-001 |
| Branch history | `docs/BRANCH_MATRIX.md` | VERIFIED for listed SHAs. 115 VentureOS remote branch names were counted, not each commit | UNKNOWN for unlisted branches |

## Tests refused or not run

Rally live workbench: not run. Playwright: not run. Postgres migrations: not applied. Paid adapters: not called. Council Lab: archive of `dec2aae` at `/tmp/cl-unit`, venv `/tmp/cl-venv`, `python -m unittest discover -s tests`. **Ran 116 tests in 9.370s, OK.** A first attempt without installing the package failed on `ModuleNotFoundError: council_lab`. That was an invocation error. The 116 passes are not a blinded-oracle acceptance and not a GitHub Actions run. `docs/NEXT.md` still says local Python does not count as the Actions check.
