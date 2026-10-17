# AutonomousOS Agent Rules

Read README.md and work-orders/ORG-CONSOLIDATE-001.md before modifying anything.

- During the first milestone, all source repositories (VentureOS, Rally, Council Lab, Engineering Runtime/FDE) are READ ONLY.
- Inventory actual remote and local branches and worktrees, including unpushed commits. Never assume the GitHub default branch represents the newest implementation.
- Record exact repository, branch, SHA, path, contract, test evidence, license and risks for each candidate.
- Do not copy source code, access secrets, merge, deploy, or modify other repositories without separate authorization.
- Do not create a competing controller/state machine until existing implementations are reconciled.
- Builders cannot approve their own work; a reviewer must inspect the full changed scope.
- An unverified capability is UNKNOWN, not complete. Missing approval means BLOCK.
- Preserve and isolate currently running product work.

Deliver a reviewed source inventory and migration decision; await approval before implementation.
