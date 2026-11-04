# Test Traceability Matrix

| Atribut | Nilai |
|---|---|
| Document Code | GOV-006 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — QA/Test Lead |
| Related Documents | `software-testing-specification.md`, `qa-acceptance-specification.md`, `../00-governance/traceability-matrix.md` |

## Test Inventory (Planned)

| Test ID | Requirement/rule | Scenario and expected result | Level | Test owner/status |
|---|---|---|---|---|
| TEST-CORE-AUTHZ-001 | SYS-AUTH-001/002; RULE-CORE-003/004 | User without permission/out-of-scope cannot read or mutate; backend denies and no data leak | API/security | TBD / Planned |
| TEST-FIN-JOURNAL-001 | RULE-FIN-001/002 | Unbalanced journal not posted; posted journal cannot be edited; correction references reversal/adjustment | Unit/integration | TBD / Planned |
| TEST-INV-MOVEMENT-001 | RULE-INV-001/002 | No direct balance mutation; transfer creates dispatch and receipt movements | Integration/E2E | TBD / Planned |
| TEST-POS-IDEMP-001 | RULE-POS-001 | Repeated checkout/payment request with same key does not duplicate side effect | API/integration | TBD / Planned |
| TEST-CORE-WF-001 | SYS-WF-001 | Invalid/stale workflow decision rejected; valid decision history persisted once | Integration/API | TBD / Planned |
| TEST-CORE-AUD-001 | SYS-AUD-001 | Sensitive action logged; password/token/secret absent from log | Security/integration | TBD / Planned |
| TEST-DATA-INTEGRITY-001 | SYS-DATA-001; RULE-DATA-001 | FK/unique integrity and migration maintain historical transaction | Integration | TBD / Planned |
| TEST-OPS-RESTORE-001 | FUN-ADM-004 | Restore DB/files to isolated environment; health and business reconciliation pass | Recovery | TBD / Planned |
| TEST-API-ERROR-001 | SYS-API-001–003 | Status/envelope/pagination/error match contract | API | TBD / Planned |
| TEST-PERF-BASE-001 | SYS-NFR-PERF-001–004 | p95 targets measured under approved hardware/data/workload | Performance | TBD / Planned |

## Execution Record Template

For each test record build/revision, environment, data seed, executed by/date, steps/evidence, actual result, pass/fail, defect ID, retest status. Protect/redact evidence. No test above has been executed by this matrix.

## Open Issues

Concrete test framework, executable test code, endpoint contracts, threshold profile, test data/credentials and reviewers not yet available. Planned IDs are placeholders and must be reconciled with module packs.

## References / Change History

DOC-030/031; GOV-005; MTS §18.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Planned test traceability inventory; no execution evidence | Documentation Agent |
