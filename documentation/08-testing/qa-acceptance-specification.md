# QA & Acceptance Specification (QAAS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-031 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — QA/Product Owner |
| Related Documents | `software-testing-specification.md`, `test-traceability-matrix.md`, `../00-governance/traceability-matrix.md`, `../01-requirements/frs.md`, `../../docs/MTS.md` |

## 1. Acceptance Governance

Acceptance owner per business domain, reviewer, evidence format, defect severity and release decision authority must be named. Agent-generated drafts are not approvals. A module remains DRAFT/IN REVIEW until humans record signoff.

## 2. Module Acceptance Criteria

A module candidate is acceptable only when:

1. Prioritized use cases agreed by Product Owner are implemented (priority source required).
2. Business rules and state transitions match approved specification.
3. Authentication, RBAC, data scope, business-state checks and negative authorization cases pass.
4. DB constraints/migrations maintain integrity and historical data policy.
5. API contract, validation, error handling, pagination/idempotency are verified.
6. Critical actions have audit trail without sensitive secrets.
7. Unit/integration/API/E2E tests for scope pass; security/performance gates meet approved thresholds.
8. User/operations docs and traceability updated.
9. No unresolved critical defect; residual risks accepted by authorized owner.

## 3. System Acceptance

System test covers core authentication/access, organization scope, workflow, cross-module data contracts, finance balance/immutability, stock movement reconciliation, POS duplicate safety, backup/restore, operational health and update path. Performance target measured only with approved load profile and environment. DR acceptance requires restore test and business reconciliation.

## 4. Review Workflow

Draft requirements → peer/owner review → test design and traceability → implementation evidence → defect triage → business/technical signoff → register status update. Rejected criteria return to owner with issue ID; scope change updates requirement IDs and impacted tests.

## 5. Open Issues

Approvers, release gate, severity levels, permitted residual risk, required test pass/coverage, accessibility, performance profile, UAT participants, production-like data policy, evidence retention and signature mechanism TBD.

## References / Change History

MTS §§18–19; DOC-030, GOV-005/006, DOC-037.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Acceptance governance and criteria based on MTS | Documentation Agent |
