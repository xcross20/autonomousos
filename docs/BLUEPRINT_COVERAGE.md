# Blueprint coverage

The governing file is the 17 October 2026 consolidation blueprint (Word, 333 non-empty paragraphs). This index maps each section to the audit page. **PROPOSED** means the architecture describes it and the code was not migrated. **UNKNOWN** means the source was not opened. Gate D sections are specified and not run.

| Blueprint section | Where it is answered | Status |
| --- | --- | --- |
| Executive decision: recover code, VentureOS owns governance, ERT only if stronger | `docs/DECISIONS.md` ADR-001, ADR-003 | PROPOSED. Founder confirmation open in `MIGRATION_DECISIONS.md` R1 and R2 |
| Evidence labels and audit rule | Ledger, this matrix, capability matrix | VERIFIED labels used. File hashes of every source file were not recorded |
| Constituent map | Inventory and ledger | VERIFIED for the four repos. Other products (TicketQuant, RapGym, Living Inheritance, HerbaGraph, PIX) were not onboarded, as the blueprint allows |
| Branch reconciliation | `docs/BRANCH_MATRIX.md` | VERIFIED for named SHAs. 115 VentureOS remote branches were counted, not read |
| One governor | ADR-002, duplication register | PROPOSED. The JSON governor is still the live one |
| Two-worker replay | Validation plan | Not run |
| Execution contract and ERT qualification | Inventory, ADR-003 | VERIFIED defect still open |
| No dual execution truth | Architecture | PROPOSED |
| Roles and department model | Prompt inventory, architecture | VERIFIED role files. Departments are labels, not services |
| RETHINK loop and permission | `rethink.ts`, ADR-004, ADR-005 | VERIFIED predicate. Apply path not executed |
| Emergence, integrity, evidence, self-improvement | Capability matrix intelligence rows | VERIFIED modules. Weighting is partial / UNKNOWN as code |
| Councils and research guardrails | Council Lab reference, R4 | VERIFIED harness exists. Blinded acceptance UNKNOWN |
| Canonical event model | Architecture event section | PROPOSED. Three stores exist today |
| Prompts and skills | `docs/PROMPT_INVENTORY.md` | VERIFIED counts and role map. Not every body |
| Operator UI and API invariants | Capability matrix API list, runbook | VERIFIED route names. UI behavior UNKNOWN. Not deployed |
| Parallel workstreams | ADR-006 | PROPOSED. Cap for real builders remains one |
| Canonical layout versus promote VentureOS | ADR-001 | PROPOSED: promote VentureOS. AutonomousOS stays the ledger |
| Bootstrap contract | `docs/BOOTSTRAP_CONTRACT.md`, runbook | PROPOSED commands differ from the real CLI. Real CLI documented |
| Job schema and state machine | Architecture | PROPOSED enum. Current code uses other words |
| Discovery steps 1–3 | Ledger, inventory, capability matrix | Done as an audit |
| Discovery step 4, tests in isolated trees | Capability matrix, QA report | Governor file 17 passed. ERT 109 passed earlier. Council result recorded in QA. Rally not run |
| Discovery step 5 freeze files | Manifest, capability matrix, duplication register, migration decisions | Present under `docs/`. Root `SOURCE_MANIFEST.yaml` points at the docs copy |
| Extraction order 1–10 | Integration plan | Not started |
| End-to-end demonstration | Validation plan | Not started. Rally sprint excluded |
| Safety, budgets, secrets | Runbook, ADR-008 | VERIFIED refusals in source. No spend numbers |
| Acceptance suite | Validation plan | Specified, not passed |
| Efficiency metrics | Efficiency baseline | Counts only. Dollars and minutes not invented |
| Phases A–F | Phase A is this audit. B–F are not authorized | Phase A still needs a human yes |
| Risk register R1–R6 | `docs/MIGRATION_DECISIONS.md` | Recommendations written. Decisions open |
| Definition of done handoff | Not met | No screenshots, no accepted slice, no cost report, no replay of a product improvement |
| Discovery checklist, every item | `docs/CAPABILITY_MATRIX.md` | Each blueprint bullet has a row. UNKNOWN stays UNKNOWN |

## Checklist items that remain UNKNOWN on purpose

- Causal outcome evaluator as a module.
- Self-improvement weighting code.
- Which of the two `010_*.sql` migrations runs first.
- Whether each web page is wired to the API.
- Playwright results.
- Full dynamic API route list beyond exact `pathname ===` matches.
- Capability of VentureOS branches that are only named in the branch list.
- A private FDE repository under another name. None was found under `xcross20` or as a local product directory.

## Items the blueprint asks for that this phase must not pretend to have

Canonical integration tests in AutonomousOS, an end-to-end evidence pack, screenshots, a one-command replay of a product slice, and a published rollback that has been executed. The rollback text is in the runbook and the integration plan. It has not been exercised.
