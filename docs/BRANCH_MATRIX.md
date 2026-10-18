# Branch matrix

Read at `2026-10-18T00:31:51Z` with local `git branch -vv` and `gh api` branch lists. No fetch and no checkout. A branch that is not in the capability table was not inspected commit-by-commit. Its capability is **UNKNOWN**. Remote counts: VentureOS 115, Rally 26, Council Lab 5, Engineering Runtime 3 (`main`, `docs/coordinator-launch-recovery-lesson`, `docs/runtime-authorization-isolation-lessons`).

Default `main` is not the newest implementation on any of these repos.

## Capability-bearing branches

| Repo | Branch | SHA observed | Where | Capability | In default main? |
| --- | --- | --- | --- | --- | --- |
| VentureOS | `feat/mission-control-v2` | `4b119d3` | primary checkout, dirty | Control plane, API, CLI, web pages used as the committed inventory | No. Local `bootstrap/ventureos-v1` is behind origin/main by 104 commits. Main itself was not diffed |
| VentureOS | `feat/background-autonomy-portfolio-governor` | `5842632` | local, tracks origin | PR16 governor introduction | Not checked. The governor code is on `4b119d3` |
| VentureOS | `feat/org-boot-001-ventureos` | live `60704c8` at this timestamp; adoption snapshot `b5a280a` | worktree, ahead 15 of origin | JSON governor and completed product jobs | No |
| VentureOS | `feat/durable-persistence-recovery` | `fcc6085` | local | PR15 Postgres recovery, by branch name and prior doc | UNKNOWN beyond the name |
| VentureOS | `feat/repo-onboarding-assimilation` | `517290a` | local | PR8 assimilator | UNKNOWN whether `4b119d3` contains every later fix |
| VentureOS | `feat/terminal-code-diff` | `2c2e5ed` | local | PR10 terminal | Package exists on `4b119d3` |
| VentureOS | `feat/ventureos-v1-v4-integration` | `2a97844` | local | Historical integration | UNKNOWN relative to mission-control |
| VentureOS | `feat/product-design-3-0-shell` and sibling design branches | several worktrees (`ventureos-pr31`, `pr32`, `pr34`) | other directories | UI shells | Dirty mission-control checkout is a different branch. Do not mix them |
| Rally | `feat/org-boot-001-rally` | live `aa304f1`; snapshot `210bbbc` | worktree, ahead 30 | Product slices the governor committed | No |
| Rally | `feat/rally-live-workbench-slice` | `3e74c6e` | primary, ahead 2, untracked docs | Live workbench. Do not touch | No |
| Rally | `feat/autonomy-bootstrap` and `autonomy/AUT-CYCLE-*` | listed locally | other worktrees | Older autonomy cycles, including the provider that uses `--always-approve` | No |
| Council Lab | `feat/cl-rel-003a-development-comparison` | `dec2aae` | local only | Comparison runner and the engineering harness | No. Remote has no such branch |
| Council Lab | `feat/cl-rel-002-scenario-integrity` | `bdaf4df` | local | Oracle-directory repair commits are ancestors of the current branch | Not on remote under this name |
| Council Lab | `main` | remote `207aaf85` | GitHub | README-level foundation at the earlier inspection | Yes, and it is behind the local work |
| Engineering Runtime | `feat/ert-002c-railway-infrastructure` | `21bac4c` | local only | Unaccepted runtime plus a blocked Railway stub | No |
| Engineering Runtime | `feat/ert-002-executable-runtime` | `2799cff` | local | Same process code, review r11 `REPAIR_REQUIRED` | No |
| Engineering Runtime | `main` | `78033ba` | tracks origin | Foundation text. Not the executable runtime | Yes |
| AutonomousOS | `feat/org-consolidate-001-foundation` | `5bffd38` before this pass | this repo | Audit documents | `main` is `4c6076dc`, scaffold only |

## Local worktrees that are not the adoption pin

VentureOS worktrees seen: `ventureos-pr29`, `ventureos-founder`, `ventureos-founder-b`, `ventureos-redesign`, `ventureos-ux`, `ventureos-pr28`, `ventureos-pr32`, `ventureos-pr34`, `ventureos-pr31`, `ventureos-poc-completion`, `ventureos-poc`, `ventureos-pr27`, and `ventureos-org-boot`. Rally has many `rally-eng-*` and harness worktrees. They were named from `git branch -vv`. Their trees were not audited. Using one of them as canonical would be a guess.

## Rule

A feature branch is evidence. It is not merged because it exists. Before any extraction, re-read `git merge-base --is-ancestor` for the exact SHA. `b5a280a` is not an ancestor of mission-control `4b119d3`.
