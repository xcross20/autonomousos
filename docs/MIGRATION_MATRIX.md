# Migration matrix

No row authorizes a copy. Gate C in `docs/INTEGRATION_PLAN.md` is still closed.

| ID | Responsibility | Strongest source | SHA | Owner after consolidation | Decision | Why | Rollback |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M1 | Job authority, budgets, leases, strategic exclusion | VOS-GOV-PORTFOLIO | `4b119d3` | VentureOS control plane | ADAPT | Only implementation with select, claim, lease, budget, pause, and strategic refusal. Default tick does not run a builder | Keep org-boot launcher as the fallback until parity tests pass |
| M2 | Proven supervised product loop | VOS-GOV-ORGBOOT | snapshot `b5a280a` | Fallback only | REFERENCE | 18 completed jobs in that blob (17 Rally, 1 VentureOS). The live queue moved afterward. Exact-file builder, one worker, JSON file | Leave the script and queue untouched |
| M3 | Role permission predicates | `packages/governance` | `4b119d3` | VentureOS | REFERENCE | `builderCannotRelease`, `reviewerCannotMutate` | No move |
| M4 | Worktree sandbox helpers | `packages/execution` | `4b119d3` | VentureOS | REFERENCE | File and git helpers, not process containment | No move |
| M5 | Bounded process lifetime | ERT `processes.py` | `21bac4c` / code `ccc29b2` | None until R11-001 is repaired and accepted | REFERENCE | Strongest killer. Documented surviving descendant. ERT-002 not accepted | Do not switch the host executor |
| M6 | Local-test evidence cycle | ERT cycle + SQLite | `21bac4c` | Engineering Runtime repo | REFERENCE | Useful contract for evidence and leases. Not certified | Stay in that repo |
| M7 | Railway / cloud execution | `execution_backend.py` | `21bac4c` | None | REJECT | `ContainmentUnverified`, zero destroy trials | Do not create Railway resources |
| M8 | Rally Grok provider | `grok_provider.py` | `210bbbc` | None | REJECT | Uses `--always-approve` | Leave in Rally history |
| M9 | Emergence | `packages/emergence` | `4b119d3` | VentureOS, on demand | REFERENCE | Separate from integrity. Weighting still partial | Do not auto-apply strategic findings |
| M10 | Councils | VentureOS `packages/councils` and Council Lab | `4b119d3` / `dec2aae` | Council Lab for research integrity. VentureOS package for product deliberation | REFERENCE | Two implementations. Neither is the engineering controller | Do not merge enums or prompts into the hot path |
| M11 | Strategic rethink | `mission-control/src/rethink.ts` | `4b119d3` | VentureOS | REFERENCE | Human approval required for mission, governance, strategy | Proposals stay unapplied |
| M12 | Repo attach | `repo-assimilator` | `4b119d3` | VentureOS | REFERENCE | Read-only scan until approval | No write on first scan |
| M13 | Operator UI and API | `apps/web`, `apps/api` | committed `4b119d3` only | VentureOS | UNKNOWN for dirty files | 29 uncommitted paths are not a source | Do not deploy the dirty tree |
| M14 | CLI entry | `apps/cli` bin `venture` | `4b119d3` | VentureOS | ADAPT later | One binary. `venture org init` was not verified to exist | No second CLI in AutonomousOS |
| M15 | Evidence hash helper | `packages/evidence` | `4b119d3` | VentureOS | ADAPT later | Too small to be the institutional log | Keep org-boot JSON and ERT SQLite until one event model is tested |
| M16 | Rally domain | workbench, labs | `210bbbc` | Rally repository | REFERENCE | Product code and live sprint | No import |
| M17 | Research integrity harness | Council Lab `state.py`, evaluation, oracle commits | `dec2aae` local | Council Lab | REFERENCE | Branch not pushed. QA of CL-REL-002 not accepted by its own NEXT file | No promotion of research results into product claims |
| M18 | Bootstrap skill and template | this repo | this branch | AutonomousOS docs only | REFERENCE | Instructions. Not a scheduler | Delete the skill rather than let it grow a queue |
| M19 | Orchestrator package | `packages/orchestrator` | `4b119d3` | VentureOS name only | ARCHIVE | Re-export of ControlPlane | Do not wrap it again |
| M20 | Canonical repository | comparison below | — | VentureOS for code. AutonomousOS for the audit | decision | See ADR-001 | AutonomousOS can be archived if the owner disagrees |

## Duplicates to stop growing

- Two governors (portfolio tick and org-boot JSON).
- Three execution launchers (`spawnSync` in org-boot, `grok_provider.py`, ERT `run_bounded`).
- Three state vocabularies (portfolio job, org-boot status, Council Lab transitions).
- Multiple `NEXT.md` / `STATE.md` files that can be edited without an event.
- VentureOS councils and Council Lab councils.
- A new AutonomousOS app layer on top of `apps/web`.

## Canonical host

VentureOS stays the canonical implementation. AutonomousOS stays the consolidation record and the contract workspace. A new runtime repository would duplicate `packages/control-plane` without inheriting its tests. The blueprint allows that choice only after comparison. The comparison is in ADR-001.
