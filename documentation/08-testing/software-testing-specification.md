# Software Testing Specification (STS-TEST)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-030 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — QA/Test Lead |
| Related Documents | `qa-acceptance-specification.md`, `test-traceability-matrix.md`, `../01-requirements/srs.md`, `../01-requirements/frs.md`, `../04-backend/api/api-specification.md`, `../../docs/MTS.md` |

## 1. Tujuan dan Scope

Menetapkan strategi verifikasi BOS. MTS mensyaratkan unit, integration, API, E2E dan security testing serta acceptance per modul. Tool, coverage threshold, environment, test data dan release gate numeric TBD.

## 2. Test Levels

| Level | Fokus | Contoh |
|---|---|---|
| Unit | Domain invariant, calculation, validation, permission policy | Journal balancing, stock movement rule, workflow transition |
| Integration | Database, repositories, transaction, queue, storage, email/external adapters | Migration/FK, outbox, import failure |
| Contract/API | AuthN/authZ, request validation, response/error, pagination, filtering, idempotency | `/api/v1` endpoint tests |
| E2E | End-to-end user process | Login, employee/leave approval, sales-to-invoice, payment/closing |
| Security | Permission bypass, IDOR, SQLi/XSS/CSRF, upload, session, rate limit, secrets exposure | Role/scope negative cases |
| Performance | MTS baseline with fixed hardware/data/concurrency | p95 read <500ms, write <800ms, dashboard <3s only in agreed test environment |
| Recovery | Backup/restore and reconciliation | Restore DB/files and validate Finance/Inventory |

## 3. Test Design and Data

Each test case has ID, requirement/rule/use case, preconditions, safe test data, steps, expected result, priority (only if agreed), environment, evidence and result. Use synthetic data; do not copy production personal/payment data without approved controls. Test unauthorized scope both collection and object access. Concurrency/idempotency tests cover numbering, journal posting, POS checkout, stock transfer, webhook consumer.

## 4. Environments and Execution

Isolate unit/integration/staging data; external provider sandbox or stub. Record app/dependency version, DB migration revision, config (no secrets), test data seed version, run ID and failures. CI gate and deployment promotion rules are defined in release management DOC-037. Flaky tests require triage; they are not silently ignored.

## 5. Defect Severity and Exit

Severity definitions, required pass percentage, coverage target, performance workload and security thresholds need QA/Product approval. Critical acceptance condition from MTS: no unresolved critical defects; requirements, authorization, constraints, API, audit, errors and documentation implemented and tests passed for module scope.

## 6. Open Issues

Test framework/runtime, CI system, supported browsers, accessibility test level, performance hardware/profile, test data classification, coverage/quality gates, severity taxonomy, external sandbox and security assessment scope TBD.

## References / Change History

MTS §§17–19; DOC-002/003, DOC-031, GOV-005/006.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Testing levels, traceability and open QA gates | Documentation Agent |
