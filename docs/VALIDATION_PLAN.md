# Validation plan

Gate D is specified here and **not run**. Gate E is specified here and **not measured** beyond the baseline notes.

A pass requires command output, SHAs, reviewer identity, and a replay recipe. Unknown is not a pass. This repository's self-description is not a pass.

## Contract tests (future, in VentureOS)

1. Portfolio selection prefers the higher score and skips strategic jobs.
2. Two workers cannot both claim one job id.
3. Overlapping scope leases: the second writer blocks.
4. Enqueue of the same idempotency key returns the original job.
5. A tick without `runPipeline` does not record complete, once the simulator tests are explicitly marked.
6. Builder role cannot record a release decision.
7. Reviewer role cannot write files.
8. Restart after a stale lease recovers one job and does not double-complete.
9. Rethink apply of `STRATEGY.md` without a human attribution fails closed.
10. Council Lab conclusion fixture does not create a portfolio job.

## Adversarial tests

- Killed governor mid-claim.
- Stale lease and a second owner.
- Partial write of the queue or event log.
- Instructions inside a product repo that tell the agent to ignore the governor.
- Review artifact whose SHA is not the candidate.
- Secret strings in captured logs.
- Detached descendant that closes fds, clears the environment, and changes directory to `/`. This is R11-001. The current ERT limitation says it survives. A pass is a dead process after `run_bounded` returns, observed with `ps`, on a commit that changes `processes.py`.
- Missing evidence treated as failure, not as accept.

## Integration pilot (Gate D), only after A–C are accepted

Use a fresh sample repository or a Rally-adjacent fixture. Do not use `/Users/immanuellewis/rally_top_signal_tester` or the org-boot branches.

Cycle:

1. Founder approves a mission file.
2. `doctor` then `init`.
3. Governor queues one scoped slice.
4. Builder commits only allowed paths.
5. Product test runs.
6. Different reviewer session reads the diff and returns ACCEPT or CHANGES_REQUIRED.
7. Emergence may propose one bounded improvement. It does not apply it.
8. Human authorizes or rejects that proposal.
9. If authorized, one more job runs and the measured result is compared to the baseline.
10. A rethink proposal is filed if the evidence contradicts the strategy. It stays unapplied.
11. Kill the governor once and show a single recovery.
12. Optional only after the above: two jobs with disjoint paths and one integration review.

Artifacts: mission, manifest SHA, events, commits, test output, review, hypothesis, measures, rethink decision, replay command.

Fail the pilot if scope widens, if a metric is fabricated, if the builder and reviewer share a session, or if a strategic file changes without a human event.

## Regression that must stay green

- Rally scanner evidence boundaries, checked in the Rally repo on its current branch, not by importing it.
- Council Lab oracle directory remains unreadable to the builder, checked in Council Lab when that QA is actually run.
- No merge to `main` on VentureOS, Rally, Council Lab, Engineering Runtime, or AutonomousOS.

## Two-worker demonstration

Not started. Required observations: overlapping execution intervals, two claim records, zero shared writable paths, one killed worker, one surviving lease, one integration review, no duplicate job completion.

## What this session verified

See `QA_REPORT.md`. Discovery documents and, if the clean-archive run finished, Engineering Runtime's existing suite. That suite does not close R11-001.
