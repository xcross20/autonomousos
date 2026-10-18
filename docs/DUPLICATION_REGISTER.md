# Duplication register

Each row is a responsibility with more than one implementation. The target owner is a proposal. Both copies stay where they are until a later authorized cutover. None of these was deleted.

| Responsibility | Copies | Conflict if both run | Target | Other copies |
| --- | --- | --- | --- | --- |
| Job selection | Portfolio `selectNext`; org-boot `selectJob` | Two jobs can be chosen for one product under different stores | Portfolio governor | Freeze the JSON selector. Do not extend it |
| Job record | `MemoryWorkQueue` / Postgres `background_jobs`; `queue.json`; Council SQLite; ERT SQLite | Success in one store is invisible to the others | Portfolio store, after a mapping test | JSON file remains the historical record of completed org-boot jobs |
| Status words | Portfolio `queued/retry_ready/running/complete`; org-boot `QUEUED/RUNNING/COMPLETED/BLOCKED`; Council `PROPOSED…VERIFIED/SAFETY_STOP` | A shared worker would mis-read state | Portfolio enum for product work. Council enum stays inside Council Lab | Map at the boundary. Do not merge the enums |
| Process launch | Org-boot `spawnSync` with `dontAsk`; Rally `grok_provider.py` with `--always-approve`; ERT `run_bounded`; `packages/agents` HTTP adapters; `packages/terminal` PTY | A child can outlive the parent, or a launch can bypass the permission rule | Org-boot `spawnSync` until R11-001 is fixed. Then reconsider ERT | REJECT `grok_provider.py` for organization launches |
| Review | Org-boot pasted files; ERT separate subprocess; `SYSTEM_REVIEWER` prompt; GitHub `evaluateMergeGate` | A narrow paste can miss an unrelated byte change | Diff-based review session that is not the builder | Merge gate stays a release check, not the code review |
| Strategy files | Root and `engineering/` copies in Rally; Council `RETHINK.md`; VentureOS `rethink.ts`; ERT `STRATEGY.md` / `NEXT.md` / `STATE.md` | An agent can edit a projection and call it authority | VentureOS rethink record plus one human approval | Product files stay projections |
| Councils | `packages/councils` and Council Lab | A research conclusion could be treated as a product job | Council Lab for blinded research. VentureOS package for optional deliberation | Neither enqueues strategic work |
| Evidence | `packages/evidence` hash helper; `/api/events`; org-boot fields on the job; ERT evidence export; Rally observed-vs-derived split | Metrics get promoted without a source | Append-only VentureOS event, later | Rally’s split stays in Rally |
| Operator UI | `apps/web` pages, CLI, org-boot stdout | The screen can show a different queue than the script | `apps/web` and `venture` CLI as clients of one API | Org-boot logs are the fallback view |
| Deploy | `packages/deploy` `RailwayDeploy`; ERT `RailwaySandboxBackend` | A stub and a live client share a name | Neither in this phase | Fixture deploy is for tests only |
| Bootstrap | Blueprint names `venture org init`. CLI help has `venture init`, which prints a hint and does not write a project | Operators follow the wrong command | Keep the real CLI names. Add commands only after Gate C | AutonomousOS skill does not enqueue |
| Worktrees | VentureOS execution sandbox, API `worktree.ts`, ERT worktrees, org-boot uses the existing checkout | Two writers in one directory | One leased worktree per job | Do not add a worktree on the live Rally sprint from this repo |

Idle is valid. A second governor that invents work to look busy is a duplicate, not a feature.
