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
- Engineering Runtime / FDE: confirm repository URL and local branches during discovery.

## Status

Bootstrap scaffold only. No production execution capability has been claimed or validated here.
