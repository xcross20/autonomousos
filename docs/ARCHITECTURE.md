# Architecture

This is the proposed shape after the audit. It is not a running system. AutonomousOS does not execute jobs.

## Decision

VentureOS owns projects, missions, authority, the queue, budgets, approvals, evidence events, and the operator clients. Engineering Runtime stays a separate repository and is not the executor until R11-001 is repaired and ERT-002 is accepted. Rally and Council Lab stay product and research repositories. AutonomousOS holds this record, the bootstrap contract, and the validation plan.

```
operator (VentureOS web, venture CLI, bootstrap skill)
        │  typed commands only
        ▼
VentureOS control plane
  projects, missions, workstreams
  one queue, leases, budgets, dependency gates
  approval and audit events
        │  job id + immutable worktree + allowed paths + timeout
        ▼
execution adapter
  today: org-boot spawnSync fallback (supervised, one job)
  not today: ERT run_bounded (blocked on R11-001)
  not today: Railway sandbox (ContainmentUnverified)
        ▼
product repository worktree and its tests
        ▼
independent reviewer (different session, full diff)
        ▼
decision event: ACCEPTED | BLOCKED | REJECTED | ESCALATE
        ▼
optional specialist proposal (emergence, rethink, council)
        ▼
human gate if the proposal changes mission, governance, or strategy
        ▼
authorized NEXT, or IDLE
```

The runtime reports events. It does not choose the next job.

## Loops, one machine

All five loops are policies on the same job record.

| Loop | What it adds | What it must not do |
| --- | --- | --- |
| Engineering | Plan, critique, build, test, review | Let the builder record ACCEPT |
| Product improvement | Emergence lenses, skeptic, saturation | Auto-apply a strategic finding |
| Strategic | Rethink proposal and critique | Edit STRATEGY or the budget without a human event |
| Research | Hypothesis, protocol, evaluation, conclusion | Turn an unvalidated signal into a product claim or a trading action |
| Self-improvement | Baseline versus a workflow change | Adopt a prompt because a model preferred it |

Workstreams (backend, frontend, PIX, research, AI, data, QA, platform) are labels, path leases, and dependency edges. They are not extra companies and not extra queues.

## Job contract (target, not implemented)

`job_id`, `project_id`, `mission_id`, `workstream`, `objective`, `scope_paths`, `excluded_paths`, `dependencies`, `authority`, `acceptance`, `budget`, `timeout`, `runtime_adapter`, `evidence_requirements`, `reviewer_profile`, `retry_policy`, `idempotency_key`.

Target transitions:

`DRAFT → APPROVED → QUEUED → CLAIMED → RUNNING → VERIFYING → REVIEW → ACCEPTED | BLOCKED | REJECTED`

`RECOVERY_PENDING`, `CANCELED`, and `EXPIRED` are distinct. `BLOCKED` does not fall through to running. Invalid transitions fail closed.

Current code does not use this enum. Portfolio jobs, org-boot statuses, and Council Lab transitions stay as they are until a mapping test exists. The mapping is a Gate C deliverable, not a rewrite done here.

## Event record (target)

Append-only: `event_id`, `project_id`, `mission_id`, `job_id`, `actor_role`, timestamp, input refs, source commit, worktree, policy version, tool version, artifact hashes, outcome, reviewer id, decision, cost, parent event.

Observed today: org-boot rewrites one JSON object; ERT uses SQLite; VentureOS evidence package hashes a blob; mission control has its own store. None of these is yet the shared log. `STATE.md` is a projection wherever it appears.

## Operator surface

One UI and one CLI read the same control-plane records. Minimum screens, already aimed at by VentureOS and not re-certified here: portfolio, onboarding, mission and workstream, queued and blocked jobs, logs, reviewer decision, approvals, cost and limits, resume.

The UI does not write job status directly. The committed shell is `4b119d3`. Uncommitted mission-control edits are outside this architecture until they have a SHA.

## Bootstrap

Specified in `docs/BOOTSTRAP_CONTRACT.md`. The skill in this repo is a checklist. It does not enqueue work.

## Parallelism

Not safe just because two agent sessions can start. Required before a second live worker: atomic claim, disjoint scope leases, separate worktrees, dependency gate, integration review, and a tested restart. First demonstration is two workers. The org-boot selector does not provide that.

## What is intentionally absent

- A second governor in AutonomousOS.
- A copy of Rally, Council Lab, or ERT.
- Docker, Railway, or GitHub Actions as a requirement to describe the system.
- Ten always-on agents. Roles are profiles invoked when the job needs them.
- Automatic merge to `main`.

## Local processes for the fallback path

The path that has already completed work needs git, Node `tsx` for the governor script, the `grok` CLI, and the product test command. It does not need Postgres. The portfolio governor's in-memory tests also do not need Postgres. A live API deployment might. That deployment was not inspected and is out of scope.

## Boundaries that stay closed

Trading, billing, customer data, health, and legal actions are not routine jobs. ERT paid launch stays refused. Rally domain experiments stay in Rally. Council Lab oracle material stays in Council Lab.
