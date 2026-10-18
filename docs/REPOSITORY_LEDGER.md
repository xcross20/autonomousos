# Repository ledger

Inspection date: 2026-10-17. Read-only. No source repository was fetched, checked out, reset, committed, or pushed by this audit.

Evidence labels: **VERIFIED** means this session read the path or GitHub API result. **REPORTED** means a file in a source repo records an earlier run that this session did not repeat. **UNKNOWN** means the object was not inspected.

The governing specification was the attached Word blueprint `autonomous_organization_consolidation_blueprint (1).docx` (333 non-empty paragraphs), not the shorter work order alone. It is not a PDF. Its remote pins were checked against local HEADs and corrected below where they differ.

## Inspected checkouts

| ID | URL | Local path | Branch | HEAD | Remote relationship | Tree |
| --- | --- | --- | --- | --- | --- | --- |
| AOS | https://github.com/xcross20/autonomousos | `/Users/immanuellewis/worktrees/autonomousos` | `feat/org-consolidate-001-foundation` | `2f61e001981df55576f587df39e6a1b6fdc34aa2` | Matches origin. `main` is `4c6076dc32c679c587cfbdf952adad01ea841854` | Clean scaffold before this audit |
| VOS-MC | https://github.com/xcross20/ventureos | `/Users/immanuellewis/ventureos` | `feat/mission-control-v2` | `4b119d39d4ad71f7ff8a96a0ca7c22537f3fc1aa` | Matches `origin/feat/mission-control-v2` | **Dirty.** 29 uncommitted paths, including mission-control API, web shell, and package sources. Those edits are not a SHA and were not used as adoption sources |
| VOS-BOOT | same | `/Users/immanuellewis/worktrees/ventureos-org-boot` | `feat/org-boot-001-ventureos` | snapshot `b5a280a916111777cc6a2e22184367788c59c99c` | At the snapshot, ahead 6 of origin `2d22aaf3f913940552bc0dd35748b3f82c3f00ad` | Clean at the snapshot. **The worktree is live.** See the drift note |
| RALLY-LIVE | https://github.com/xcross20/rally_top_signal_tester | `/Users/immanuellewis/rally_top_signal_tester` | `feat/rally-live-workbench-slice` | `3e74c6ea0d84bc734deeccb5b20c86c6be8d45de` | Ahead 2 of origin at the snapshot | Untracked `docs/engineering/` and `docs/strategy/`. Not read as product authority and not modified |
| RALLY-BOOT | same | `/Users/immanuellewis/worktrees/rally-org-boot` | `feat/org-boot-001-rally` | snapshot `210bbbca6ef4b7cfed148bc31b03d8ca061f4cda` | At the snapshot, ahead 12 of origin `c1a8c1232e67a7bcc03cd891569ecaf9eec22eca` | Clean at the snapshot. **The worktree is live.** See the drift note |
| CL | https://github.com/xcross20/Council_lab | `/Users/immanuellewis/worktrees/Council_lab` | `feat/cl-rel-003a-development-comparison` | `dec2aae8d161935fa012dc556d93274f9ba9c453` | **No upstream.** Not on the remote branch list | Clean |
| ERT | https://github.com/xcross20/engineeringruntime | `/Users/immanuellewis/worktrees/engineeringruntime` | `feat/ert-002c-railway-infrastructure` | `21bac4c9ba24db6b47c433440ddb1bf071ca634f` | **Local only.** `origin/main` is `78033ba22bf1a719821c2b6a2ae12b188e0eef72` | Clean |

`b5a280a` is not an ancestor of VOS-MC `4b119d3`. The org-boot governor and the mission-control branch are divergent lines.

## Live drift after the snapshot

The org-boot governor kept committing while this audit was open. These later reads are observations, not a new frozen pin. Do not treat them as the adoption SHA.

| When | VOS-BOOT | Queue file | RALLY-BOOT |
| --- | --- | --- | --- |
| First read (snapshot) | `b5a280a`, clean, ahead 6 | committed blob: `maxAssignments` 18, 18 jobs, all `COMPLETED`. 17 Rally, 1 VentureOS (`org-boot-003-ventureos-limit`) | `210bbbc`, clean, ahead 12 |
| Independent review | `f4978f1`, ahead 11 | 27 jobs: 23 `COMPLETED`, 3 `QUEUED`, 1 `RUNNING` | `d89fd6b`, ahead 22 |
| Author re-read after that review | committed `826f1226644b52e2607240c20d0a744533722d96`, ahead 12, **dirty** `docs/org-boot-003/queue.json` | working tree at that moment: max 27, 24 `COMPLETED`, 3 `QUEUED` (`rally-sprint-006-liquidity-volume`, `rally-sprint-006-metric-route`, `rally-sprint-006-metric-page`), none `RUNNING` | `0ec92b387f77b2b2fedc01d6239ee1008f8395c1`, ahead 24, dirty `src/workbench/contract_watch.py` and `tests/workbench/test_contract_watch.py` |

This audit did not start, stop, or edit that governor. Rally's sprint was left alone. Any SHA above can already be stale.

## Engineering Runtime identity

No directory named FDE was found as a product repository. GitHub repository search under `xcross20` returned no repository named `fde`. The runtime that contains bounded process execution is **engineeringruntime**. A private repository under another name remains **UNKNOWN**.

Local ERT branches:

- `main` `78033ba` — foundation text, tracks origin
- `feat/ert-001-harness-audit` `1a83d57` — research audit accepted administratively
- `feat/ert-002-executable-runtime` `2799cff` — runtime not accepted
- `feat/ert-002c-railway-infrastructure` `21bac4c` — current local HEAD

`processes.py` last changed in `ccc29b2`, which is an ancestor of review candidate `ef4fda796c03ed0ffc7104836849c03789c237be`. Commits after that SHA do not change the process killer.

## Council Lab other worktrees

Sampled, not fully audited:

| Path | Branch | HEAD |
| --- | --- | --- |
| `/Users/immanuellewis/worktrees/cl-aut-007-demo` | `main` | `034b012` |
| `/Users/immanuellewis/worktrees/cl-aut-007-repair` | `main` | `b0be844` |

Remote Council Lab branches seen: `main` (`207aaf858b7fd44d0c1cc68b374d213983f0a878`), `feat/cl-001-foundation`, `feat/cl-rel-001-fault-simulator`, `docs/cl-002-software-repair-research`, `docs/cl-aut-operating-model`. The development-comparison branch and the CL-AUT feature branches in the local checkout are **local-only**.

## VentureOS package roots verified on `4b119d3`

`packages/`: agents, asdos, canary, computer, config, control-plane, councils, db, deploy, domain, emergence, evidence, execution, github, governance, institutional-state, mcp-registry, mission-control, orchestrator, padp, preview, prompts, realtime, repo-assimilator, schema-guard, skills, telemetry, terminal.

`apps/`: api, cli, staging-probe, web.

`prompts/`: `v1`, `v3`, plus `V1_SLICE_0_1_CODING_AGENT.md`. Council Lab also keeps `prompts/original-v3`, and its README says those 27 prompts are not enabled by the CLI.

## Licensing

No `LICENSE` or `COPYING` file was found at the top of VentureOS, Rally, Council Lab, or Engineering Runtime. VentureOS `package.json` is `"private": true`. Copying packages into AutonomousOS is not licensed by this audit. REJECT and ARCHIVE rows still do not grant a copy. REFERENCE and ADAPT rows also stay in the source repo until the owner grants one.

## What this session did not do

- Did not read every historical VentureOS branch commit-by-commit. Branch names in the blueprint were treated as candidates. The two HEADs above are the ones with current worktrees.
- Did not run VentureOS vitest. The primary tree is dirty, and the org-boot tree is the live product queue.
- Did not run Council Lab or Rally tests inside their checkouts.
- Did not call a paid model, Railway create/deploy, or GitHub merge.
