# Efficiency baseline

No new benchmark was run. Numbers below are counts this session read. Times, token totals, and dollar costs that were not stored are omitted on purpose.

## What exists today

The working supervised path is `scripts/org-boot-resume.sh` launching `scripts/org-boot-governor.ts`. Counts below are the committed queue blob at snapshot `b5a280a`, not the working tree after the governor kept running.

| Measure | Observation | Source |
| --- | --- | --- |
| Jobs in the snapshot queue | 18, all COMPLETED (17 Rally, 1 VentureOS) | `queue.json` at `b5a280a` |
| Strategic jobs in that blob | 0 | same blob |
| Same file after the snapshot | Moved. A post-review read saw a dirty queue with queued sprint-006 jobs. Not a baseline number | `docs/REPOSITORY_LEDGER.md` |
| Workers | 1. `selectJob` returns a single job | `org-boot-governor.ts` |
| Builder timeout | 300000 ms `spawnSync` | same file |
| Test timeout | 180000 ms | same file |
| How a job is specified | Full file contents are in the queue. The prompt says to write those contents exactly | `writeBuilderPrompt` |
| Cost field | Not present on the job record | `queue.json` schema as read |
| Services for that path | git, Node tsx, grok CLI, the product test command. No database | script imports |
| Portfolio governor default | Marks `complete` with no builder when `runPipeline` is omitted and `simulateFailure` is empty. A non-empty `simulateFailure` throws | `worker-loop.ts` |
| Portfolio parallel default | `VENTUREOS_MAX_PARALLEL` default 4, in-memory tests only | `governor.ts`, PR16 doc |
| Paid Grok from ERT | Refused. No hard ceiling flag | ERT README and limitations |
| Railway processes | Not created. Containment trials 0 | ERT-002C decision record |
| Bootstrap commands | Not implemented here | this repo before and after the docs |
| Human preparation | A person authors the queue files, the exact contents, the test argv, and the review rule | job type in the governor |

## Qualitative baseline

Time to a new project is not one command. Someone creates a queue entry by hand, points it at a worktree and a branch, and runs the resume script. The builder is constrained to the supplied file text. That is reliable for small supervised edits and expensive in human effort for an open-ended slice. Review is a second model call over pasted files, not a measured diff gate.

Duplicate execution risk: a second governor, if started, would be a different mechanism (`MemoryWorkQueue` versus the JSON file) and could claim different work under the same product name. They are not running as one system.

Context cost of this audit: the blueprint, the stub repo, and targeted source reads. Full package bodies for telemetry, db, and institutional-state were not loaded. That was intentional.

## Target metrics (collect later, do not invent now)

- Minutes from approved mission to first accepted user-visible slice.
- Human edits per accepted job. The current path's human edits include the entire file body.
- Cost per accepted job, from a ledger the governor writes.
- Reviewer findings that the builder's test did not catch.
- Recovery success after a killed process.
- Duplicate completions.
- Number of local services. Target for the fallback remains the current set until Postgres is actually required.

## Saturation

Do not add councils, a cloud sandbox, or a new UI to the first kernel in order to look complete. Add one only when a recorded job shows the current chain cannot express the requirement.

## Comparison method for Gate E

After the kernel exists, run the same small mission two ways: the hand-written org-boot job, and `venture org init` plus one governed job. Record wall time, human interventions, test failures, and whether a secret appeared in a log. One pair is enough. A model-written estimate is not a result.
