# Event & Queue Specification (EQS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-020 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Backend/Operations Owner |
| Related Documents | `backend-technical-specification.md`, `../02-architecture/integration-architecture.md`, `../09-operations/monitoring-logging.md`, `../../docs/MTS.md` |

## 1. Tujuan dan Baseline

Redis-based queue menurut MTS untuk email, notification, report generation, file processing, import/export, event handler, dan scheduled jobs. Job membutuhkan retry, timeout, logging dan failure handling. Exact Laravel queue driver/version belum disahkan.

## 2. Event vs Job

- **Domain event:** fakta domain setelah perubahan terjadi; consumer tidak boleh mengubah fakta asal seolah belum terjadi.
- **Application job:** instruksi pekerjaan asynchronous, dapat mempunyai retry/status.
- **Integration event:** kontrak lintas boundary/provider yang versioned dan memiliki data minimum.

Nama, payload, versioning, PII policy, correlation ID, occurred-at, causation ID dan owner harus ditetapkan sebelum event dipublikasikan.

## 3. Candidate Workloads

| Workload | Trigger | Idempotency/recovery concern |
|---|---|---|
| Email/in-app notification | Workflow/business event | Delivery dedupe, retry, provider error |
| Report generation | User/schedule request | Status/polling, authorization recheck at download |
| File/import/export processing | Upload/request | File validation, partial row failures, retry safety |
| Cross-module event handlers | Committed domain change | Outbox/durable publish, consumer dedupe |
| Scheduled task | Scheduler configuration | Overlap prevention, missed-run policy TBD |
| External webhook | Provider event / outbound subscription | Signature, idempotency, retry and delivery log |

## 4. Reliability Contract

- Dispatch after durable DB commit; important events evaluate transactional outbox. Exact scope/outbox technology TBD.
- At-least-once delivery must be assumed unless implementation proves otherwise; handlers idempotent and side effects deduplicated.
- Per job define timeout, max attempts, backoff, retryable/permanent error, failure/dead-letter handling, operator action, alert, and data retention.
- Avoid duplicate email/payment/stock/ledger side effects on retries; use business idempotency keys and unique constraints where justified.
- Trace with request/correlation ID; logs redact sensitive values.
- Queue outage behavior: reject, store pending, or synchronous fallback is use-case-specific; do not silently lose required work.

## 5. Operations and Security

Separate worker and scheduler process per MTS; least-privilege DB/storage/provider credentials; protect Redis network access; monitor queue depth, oldest age, retry/failure, worker heartbeat and scheduler heartbeat. Thresholds, dashboard, alert destinations, retention and worker concurrency TBD. Production queue administration follows DOC-033/038.

## 6. Open Issues

Queue driver/topology, outbox mechanism, job naming/payload schema, retry/backoff/timeout defaults, DLQ/failed job store, scheduling overlap/misfire policy, ordering guarantees, rate limits, Redis persistence/HA, monitoring thresholds and PII retention.

## References / Change History

MTS §§3.2, 4.4, 5.6, 8.6, 11, 15–16; DOC-018, DOC-011.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Baseline event/job reliability and operation requirements | Documentation Agent |
