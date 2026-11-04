# Backend Technical Specification (BTS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-018 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Backend Architect |
| Related Documents | `../02-architecture/sad.md`, `../02-architecture/module-architecture/module-architecture-specification.md`, `../03-data/database-design.md`, `api/`, `event-queue-specification.md`, `authentication-authorization.md`, `../../docs/MTS.md` |

## Tujuan dan Baseline

Menetapkan konvensi backend Laravel/PHP modular monolith dan REST JSON API. Laravel/PHP tercantum MTS sebagai stack; ADR-004 berstatus Proposed dan versi final runtime TBD.

## Struktur dan Responsibility

Logical module structure: Domain (entities/value objects/services/events/exceptions), Application (commands/queries/DTO/use cases), Infrastructure (persistence/repositories/integrations), Presentation/Http (controllers/requests/resources). Menerapkan Laravel conventions diperbolehkan selama boundary domain terjaga. Controller tipis, dependency injection, Form Request validation, Policy/authorization service, application layer untuk use case kompleks.

## Request Pipeline

1. Routing `/api/v1` dan request/correlation ID.
2. Rate limit/authentication middleware.
3. Permission, scope, business-state authorization di backend.
4. Request validation, normalization, DTO.
5. Application use case mengorkestrasi domain dan transaction.
6. Repository/persistence mengubah data owned module dengan constraint.
7. Audit/domain event/outbox sesuai kebutuhan transaksi.
8. Resource/standard response atau mapped domain/validation error.

## Transaction and Persistence

Multi-table invariants menggunakan DB transaction; transaction boundary dimiliki application use case. Jangan melakukan network call ke provider sambil menahan transaksi lama tanpa kebutuhan/keputusan khusus. Migration versioned dan reviewed; constraints ditegakkan di domain/database sesuai peran. Idempotency untuk payment/POS checkout/journal posting/stock transfer/webhook. UUID/ULID, exact ORM/schema and package versions TBD.

## Cross-Module Interaction

Public application service/query/event contract; no direct cross-module writes. Contract versioning and ownership DOC-011/012. Important event publication considers transactional outbox. Shared logic must have named owner and avoid duplicated domain rules.

## Error/Observability

Map validation to 422, unauthenticated 401, forbidden 403, not found 404, conflict 409, rate limit 429, and internal 500 per MTS. Do not expose stack traces/secrets. Capture request ID, module, operation, status, duration, queue correlation. Redact personal/payment/credential data according to yet-to-be-approved classification.

## Testing/Quality

Unit domain policies; integration DB/repository/queue/storage; API auth/validation/response/idempotency; module contract tests; security and E2E per DOC-030. Runtime version, lint/static-analysis tools, test framework configuration and coverage threshold TBD.

## Open Issues

Laravel version/PHP version, auth package/session strategy, repository/ORM patterns, exact outbox implementation, API error details, logging redaction and retention, multi-module transaction boundaries.

## References / Change History

MTS §§3–4, 8–10, 13, 18–19; DOC-007/010/011/013/019/020/021.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Baseline backend structure and runtime flow | Documentation Agent |
