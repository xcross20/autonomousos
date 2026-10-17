# Architecture — consolidation hypothesis

This describes proposed boundaries, not a claim of integrated functionality.

ChatGPT / bootstrap skill / CLI / operator UI
→ VentureOS-style Control Plane (organization, workstreams, authority, queue, approvals, dependencies, leases, evidence)
→ versioned Execution Contract
→ ERT/existing runtime adapter (bounded process, worktree, recovery, logs)
→ product repositories and tests
→ independent review and evaluation
→ emergence/evidence/RETHINK proposal
→ authority gate → next authorized work.

## Principles

1. One authoritative job state and approval model.
2. UI and skills are clients, never the authority for job state.
3. Execution runtime handles process lifecycle, cancellation, containment and recovery, not product prioritization.
4. Councils and emergence produce proposals/evidence; strategic scope expansion requires separate authorization.
5. Parallel work requires atomic claims, isolated worktrees, compatible scope leases and integration review.
6. Idle is valid; prevent infinite agent loops and fabricated progress.
7. Reuse strongest verified components, not a new monolithic rewrite.

## Hosting decision remains open

After auditing existing implementations, compare keeping VentureOS canonical versus making this repository canonical. This repository is initially a clean integration workspace.
