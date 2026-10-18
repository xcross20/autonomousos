# Operator runbook

Current fallback only. This does not start jobs. It does not replace the live governor. AutonomousOS does not execute work.

## What is running

The supervised loop is `scripts/org-boot-resume.sh` in `/Users/immanuellewis/worktrees/ventureos-org-boot`, which runs `scripts/org-boot-governor.ts` against `docs/org-boot-003/queue.json`. It selects one approved, non-strategic job, launches `grok` with `--permission-mode dontAsk`, runs the job’s test command, then a reviewer session. It refuses `main` and `--always-approve`.

Do not start a second copy. Do not point it at `/Users/immanuellewis/rally_top_signal_tester` (the live workbench checkout). The org-boot Rally worktree is the one the queue already uses.

Idle is success when every approved job is complete or blocked. Do not add a job to make the queue look active.

## Commands that exist

Verified in `apps/cli/src/index.ts` at `4b119d3`. The binary name is `venture`. It calls `VENTUREOS_API_URL` or the config API. It is not the org-boot script.

| Command | What the source does |
| --- | --- |
| `venture init` | Prints `{ ok: true }` and tells the operator to start `npm run dev:api`. It does not write `MISSION.md` or register a project |
| `venture doctor` | `GET /health` and `GET /api/providers` |
| `venture status` | `GET /api/snapshot` |
| `venture project list` / `add` / `assimilate` / `bootstrap` | HTTP to `/api/projects` |
| `venture work create` / `list` / `watch` / `pause` / `resume` / `stop` / `green` | HTTP to work-cell routes. `green` posts `.../green-loop` |
| `venture work start <cell>` | Implemented. Not in the help text. `POST /api/work-cells/<id>/start`. This is not `venture work start --mission` |
| `venture approvals approve` / `deny` | HTTP to `/api/approvals` |
| `venture deploy staging` | HTTP. Not authorized by this audit |

`venture org init` is not in the CLI. `venture work start <cell>` exists and starts a work cell by id. The contract’s `venture work start --mission` does not. Do not treat the existing handler as project bootstrap.

`engineering-runtime plan` prints `DRY_RUN_ONLY`. `engineering-runtime` modes other than `local-test` are refused. `backend` returns exit 2 and does not launch Railway.

## Gates

| Action | Allowed now |
| --- | --- |
| Read source, `git status`, `gh` read | Yes |
| Edit Rally, the org-boot queue, or ERT | No |
| Push AutonomousOS docs branch | Only the consolidation branch, never `main` |
| Merge any `main` | No |
| Paid model call | No |
| `railway up`, sandbox create, or live `createDeploy` | No |
| Strategic file change | No, unless a human approval event exists |
| Trading or billing action | No |

## Pause and recovery

Org-boot has no pause flag in the script that was read. Stop the process and leave the queue file for the running governor to finish its own write. Do not delete `queue.json`. Portfolio pause is `POST /api/governor/pause` on a running API. This pass did not call it.

If a claim is stale, `MemoryWorkQueue.recoverStale` sets `retry_ready` after `leaseExpiresAt`. That is in-memory unless a host persists it. Org-boot recovery is “run the script again”; a `RUNNING` job is selected again. A completed job with a builder session is not relaunched by the script’s own rules as previously read. Re-read the script before relying on that after `b5a280a`, because the live file has moved.

Rollback of a future kernel is: stop the new tick, leave `scripts/org-boot-resume.sh` as the launcher, and do not rewrite completed JSON records.

## Secrets

Do not put tokens in prompts, the manifest, or logs. `packages/github` exports `redactSecrets`. ERT redaction is a denylist. Neither is proof that every secret shape is covered.

## When something is wrong

Stop. Record the SHA, the command, and the exit code. Do not deploy to hide a failed test. R11-001 is a stop for any ERT adoption. A dirty mission-control tree is a stop for using unpublished UI as the operator surface.
