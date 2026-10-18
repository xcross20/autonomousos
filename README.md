# AutonomousOS

AutonomousOS is the consolidation workspace for a reusable autonomous product-engineering organization.

**Phase 0: recover, inventory, and reconcile existing implementations before moving code.** This repository does not yet constitute a functioning governor or execution runtime.

## Mission

Make a new or existing product repository bootstrappable into a bounded, auditable cycle: strategic mission → authorized vertical slice → isolated builder → independent verification → evidence-backed decision → product evaluation / rethink → next authorized action.

## Design rules

1. Reuse verified implementations; do not rewrite working VentureOS, ERT, Rally, or Council Lab components without evidence.
2. One authoritative governor for queue state, permissions, budgets, dependencies and approvals; one well-defined execution runtime boundary.
3. Keep strategy and research authority distinct from code execution. A proposal is not authorization.
4. Every migrated component has source repository, branch, commit, path, license, owner, tests, and adopt/adapt/reference/archive decision.
5. Work on feature branches, use independent review, and never merge, deploy, or access production secrets automatically.
6. Preserve existing product workflows while consolidation proceeds.

## Planned structure (directories appear as files are created)

- `docs/` — architecture, inventory, migration matrix, decisions, verification
- `work-orders/` — bounded assignments and acceptance gates
- `skills/bootstrap-organization/` — reusable bootstrap instructions (after inventory)
- `templates/product-repository/` — minimal project contract (after inventory)
- `packages/` — selectively recovered control-plane/runtime/evaluation modules (after approval)
- `apps/cli/` and `apps/web/` — minimal operator interfaces (after approval)
- `tests/integration/` — end-to-end demonstrations
- `scripts/` — inventory and migration helpers

## First milestone: ORG-CONSOLIDATE-001

Inventory the source-of-truth implementations and prove their provenance; then choose a canonical hosting architecture. Do not bulk-copy code yet. See `work-orders/ORG-CONSOLIDATE-001.md` on the setup branch.

## Source candidates

- https://github.com/xcross20/ventureos
- https://github.com/xcross20/rally_top_signal_tester
- https://github.com/xcross20/Council_lab
- https://github.com/xcross20/engineeringruntime (no separate FDE repository was found)

## Status

Phase 0 inventory is written on this branch. Independent review verdict: **CHANGES_REQUIRED** (`QA_REPORT.md`). The false pins from that review were corrected in the ledger and were not re-reviewed. The organization has not been built, migrated, or deployed. Gate D has not started. The Rally and VentureOS org-boot worktrees were live during the audit and must not be treated as frozen.

Read, in order:

- `docs/REPOSITORY_LEDGER.md`
- `docs/COMPONENT_INVENTORY.md`
- `docs/MIGRATION_MATRIX.md`
- `docs/ARCHITECTURE.md`
- `docs/DECISIONS.md`
- `docs/INTEGRATION_PLAN.md`
- `docs/BOOTSTRAP_CONTRACT.md`
- `docs/VALIDATION_PLAN.md`
- `docs/EFFICIENCY_BASELINE.md`
- `docs/SOURCE_MANIFEST.yaml`
- `QA_REPORT.md`

Canonical code host proposed in ADR-001: VentureOS. This repository is the audit. Engineering Runtime is `https://github.com/xcross20/engineeringruntime`. ERT-002 is not accepted. R11-001 is unresolved. Do not deploy.
