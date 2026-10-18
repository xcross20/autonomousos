# Bootstrap contract

Status: specification. The command names are a target for the VentureOS `venture` CLI. They are not implemented in AutonomousOS, and this session did not find a verified `venture org init` handler.

## Operator outcome

A person can point at a new or existing repository, approve a mission, and start one bounded engineering job whose result is a user-visible slice, a test, and an independent review. They do not need to know branch names inside VentureOS.

## Inputs

- Repository URL or local path.
- Mission file: user, problem, first journey, acceptance check, non-goals.
- Budget ceiling and wall-time limit.
- Protected branches. `main` is protected by default.
- Credential policy. Default is no secrets in prompts, logs, or the job record.
- Strategic ceiling. Default is proposal only.

Missing input is a blocker, not a guessed mission.

## Project files the template may create after approval

- `AGENTS.md` — permissions. Builder does not accept. Reviewer does not mutate.
- `.ventureos/project.yaml` — identity, runtime adapter name, budget, protected branches. Schema id `autonomousos.project/v0`.
- `docs/MISSION.md`
- `docs/PRODUCT.md` — first journey and acceptance.
- `docs/STRATEGY.md`
- `docs/RETHINK.md` — triggers only. Empty of decisions until one exists.
- `docs/NEXT.md` — the single authorized job, or an explicit idle statement.
- `docs/STATE.md` — generated projection. Hand edits are not authority.

Research ventures may add `docs/RESEARCH_PROTOCOL.md`. The engineering loop does not require it.

## Commands

```
venture org doctor --repo <path>
venture org init --repo <url-or-path> --mission <file> --template product
venture work start --mission first-slice
venture work status --follow
```

`doctor` is read-only: branch, dirty tree, license, test command, secrets that would be inherited, and whether a governor is already registered.

`init` is idempotent. A second run does not create a second project, does not overwrite `STRATEGY.md`, and does not enqueue a duplicate `idempotency_key`. Existing repositories are scanned by `doctor` first. Writes require an explicit approval flag that this contract does not turn on by itself.

`work start` enqueues one approved non-strategic job through the portfolio governor. It does not shell out to a private queue. It refuses to start when R11-001 is still the selected runtime, when the repo is `main`, when the tree is dirty in the allowed paths, or when the budget is unset.

`work status` prints the durable state, the blocker, and the next legal action. IDLE is a valid printout.

## First slice

The first job's acceptance is a check the product repo can run without a paid API, plus a reviewer session that is not the builder session and that receives the diff. A browser journey is required when the slice is user-visible. The contract does not invent the slice. The mission file names it.

## Recovery

If the governor process dies after claim, the lease expiry returns the job to `RECOVERY_PENDING` and then to queued or blocked with the same idempotency key. It does not start a second builder while the first lease is live. Success is not inferred from process exit 0. The decision event has to exist.

## Context loading

Pass the mission, the job scope, and hashes of the evidence the job names. Do not reload the blueprint, the full VentureOS tree, or unrelated prompts. Repo scans are cached by commit SHA.

## Skill

`skills/bootstrap-organization/SKILL.md` teaches an agent to follow this contract. The skill has no queue and no permission to launch `grok`, spend money, or edit a product repo.
