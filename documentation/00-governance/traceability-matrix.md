# Requirements Traceability Matrix

| Atribut | Nilai |
|---|---|
| Document Code | GOV-005 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Requirements Manager |
| Related Documents | `document-master-register.md`, `../01-requirements/brs.md`, `../01-requirements/srs.md`, `../01-requirements/frs.md`, `../08-testing/test-traceability-matrix.md` |

## Tujuan

Mencatat keterlacakan awal BRS ke system/functional requirements, use cases/process/rules, modul pemilik, serta test yang direncanakan. Trace ke API/entity adalah kandidat domain dokumen hingga kontrak final ditetapkan.

## Matrix

| Business Req | System/Functional Req | Use case / process / rule | Owner module | Design target | Verification target | Status |
|---|---|---|---|---|---|---|
| BUS-CORE-001 | SYS-ID-001; FUN-ID-001 | UC-CORE-001 | Core/Identity | DOC-021, DOC-019 | TEST-CORE-AUTH-001 (planned) | Draft |
| BUS-CORE-002 | SYS-ORG-001; FUN-ORG-001 | RULE-CORE-001 | Organization | DOC-013–015 | TEST-CORE-ORG-001 (planned) | Draft |
| BUS-ACCESS-001 | SYS-AUTH-001/002; FUN-AUTH-001/002 | UC-CORE-002; RULE-CORE-003/004 | Access Control | DOC-021/027 | TEST-CORE-AUTHZ-001 (planned) | Draft |
| BUS-WORK-001 | SYS-WF-001; FUN-WF-001 | UC-WF-001; BPS-WF-001 | Workflow | DOC-022, DOC-020 | TEST-CORE-WF-001 (planned) | Draft |
| BUS-AUD-001 | SYS-AUD-001; FUN-AUD-001 | RULE-CORE-005 | Audit | DOC-010, DOC-013 | TEST-CORE-AUD-001 (planned) | Draft |
| BUS-DATA-001 | SYS-INT-001; SYS-DATA-001; FUN-DOC/NUM/CF | RULE-MOD-001, DATA-001, NUM-001 | Shared Core/Data | DOC-010–017, DOC-019 | TEST-DATA-INTEGRITY-001 (planned) | Draft |
| BUS-FIN-001 | SYS-FIN-001; FUN-FIN-001/002 | UC-FIN-001; BPS-FIN-001; RULE-FIN-001–004 | Finance | DOC-013–015, DOC-019 | TEST-FIN-JOURNAL-001 (planned) | Draft |
| BUS-INV-001 | SYS-INV-001; FUN-INV-001/002 | UC-INV-001; BPS-INV-001; RULE-INV-001–003 | Inventory | DOC-011–015 | TEST-INV-MOVEMENT-001 (planned) | Draft |
| BUS-POS-001 | SYS-POS-001; FUN-POS-001 | UC-POS-001; BPS-POS-001; RULE-POS-001 | POS | DOC-011, DOC-019/020 | TEST-POS-IDEMP-001 (planned) | Draft |
| BUS-HRM-001 | FUN-HRM-001/002 | Employee/leave/payroll use cases TBD | HRM | Module pack planned | TEST-HRM-LIFECYCLE-001 (planned) | Draft |
| BUS-REPORT-001 | SYS-REP-001; FUN-ANL-001 | Report use cases TBD | Analytics | DOC-011/013, module pack planned | TEST-ANL-AUTH-001 (planned) | Draft |
| BUS-OPS-001 | SYS-OPS-001; FUN-ADM-001–004 | UC-ADM-001; BPS-OPS-001 | System Administration | DOC-028/029, DOC-032/033/038 | TEST-OPS-RESTORE-001 (planned) | Draft |
| BUS-INTEG-001 | SYS-INT-001; FUN-SALES-001 | BPS-POS-001; module use cases TBD | Cross-module | DOC-011/019/020 | TEST-INTEG-CONTRACT-001 (planned) | Draft |

## Gap/Change Control

- `planned` test IDs are placeholders for test planning and must be assigned in DOC-030 before execution.
- API/resource/entity mapping remains coarse until DOC-010–022 and module packs are reviewed.
- Requirements without approved scope, owner or acceptance are not considered implementation-ready.
- Requirement change must update linked use cases, rules, design, tests and acceptance evidence.

## Open Issues

Module pack/requirement expansion, priority, test IDs, version baselines, trace owner and approval workflow TBD. This matrix is initial and not complete enough to claim full traceability.

## References / Change History

DOC-001–006, DOC-030/031; `AGENT.md` §§9, 15, 17.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Initial BRS-to-requirement/design/test matrix | Documentation Agent |
