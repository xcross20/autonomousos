# Prompt and skill inventory

Counts are from `git ls-tree` at VentureOS `4b119d3` and Council Lab `dec2aae`. Individual prompt bodies were not all opened. None is certified by a golden test in this pass. Owner of the VentureOS set is VentureOS. Disposition of every row is **REFERENCE**. The AutonomousOS bootstrap skill is instructions and is not part of `PromptRegistry`.

## Enabled by the VentureOS registry

`packages/prompts/src/index.ts` maps roles to files:

| Role | File |
| --- | --- |
| portfolio_orchestrator | `prompts/v1/PORTFOLIO_ORCHESTRATOR.md` |
| planner | `prompts/v1/PLANNER.md` |
| critic | `prompts/v1/CRITIC.md` |
| builder | `prompts/v1/BUILDER.md` |
| system_reviewer | `prompts/v1/SYSTEM_REVIEWER.md` |
| release_judge | `prompts/v1/RELEASE_JUDGE.md` |
| incident_diagnostician | `prompts/v1/INCIDENT_DIAGNOSTICIAN.md` |
| scope_guard | `prompts/v1/SCOPE_GUARD.md` |
| portfolio_reporter | `prompts/v1/PORTFOLIO_REPORTER.md` |
| e2e_verifier | same file as system_reviewer |
| specialist_auditor | same file as system_reviewer |

`e2e_verifier` and `specialist_auditor` do not have their own prompt file. That is a real limit, not a second reviewer.

Emergence names loaded by `prompts.emergence` include `EMERGENCE_GOVERNOR`, `REFERENCE_CLASS_CURATOR`, `EMERGENCE_SKEPTIC`, `EMERGENCE_SATURATION_JUDGE`, `EMERGENCE_IMPLEMENTER`, `UX_RESEARCHER`, and `VISUAL_DESIGN_DIRECTOR`. The full map is lines 77–110 of `packages/prompts/src/index.ts` at `4b119d3`. `prompts/v3` contains 60 files. `prompts/v1` contains 11 files.

Duplicate copies of the V1 role prompts also sit under `docs/03_V1_CONTROL_PLANE/prompts/`. Council prompt copies sit under `docs/05_V3_COUNCILS/prompts/councils/`. The registry paths are `prompts/v1` and `prompts/v3`, not the docs copies. Prefer the registry path if they diverge. This pass did not hash-compare them.

## Skills

Nineteen `SKILL.md` files under `.agent/skills/` at `4b119d3`, including `release-readiness`, `security`, `design-qa`, `user-journey-e2e`, `persistence-migration`, and `clean-code`. `packages/skills` classifies mutations and points at `.agent/skills/<id>/SKILL.md`. They are audit checklists, not schedulers.

## Stored and not enabled

Council Lab `prompts/original-v3/` has 29 paths at `dec2aae`, including archetypes, briefs, and deliberation prompts. `README.md` in that repo says the 27 authentic V3 prompts are not enabled by the CLI. The tree has 29 paths because of `FILES.json` and `README.md` plus the prompt files. This pass did not reopen the README’s arithmetic. The CLI does not load them. **REFERENCE**, stored-only.

Rally `scripts/research`, `scripts/strategy_lab`, and `scripts/autonomy` are product scripts, not entries in `PromptRegistry`. **REFERENCE**. Do not add them to the organization skill list.

## AutonomousOS

`skills/bootstrap-organization/SKILL.md` tells an agent to wait for Gate C and to call VentureOS. It has no prompt registry and no queue.
