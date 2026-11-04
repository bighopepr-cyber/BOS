# Monitoring & Logging Specification

| Atribut | Nilai |
|---|---|
| Document Code | DOC-038 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — SRE/Operations Owner |
| Related Documents | `../06-security-infrastructure/infrastructure-deployment.md`, `../06-security-infrastructure/security-specification.md`, `system-administration-manual.md`, `troubleshooting-guide.md`, `../../docs/MTS.md` |

## Tujuan dan Signals

MTS expects health checks, app/auth/audit/queue/integration/system logs and admin dashboard signals: app version, DB connection, queue, storage, disk, workers, scheduler, recent errors, last backup. Tool/retention/thresholds not selected.

## Metrics/Logs

| Signal | Indicative fields | Use |
|---|---|---|
| Request | duration/status/route template/request ID | latency/error rate; avoid raw query/payload |
| Database | availability/connection/latency/storage | health/capacity; no credentials |
| Queue | depth/oldest age/failure/retries/worker heartbeat | detect backlog and stuck jobs |
| Scheduler | heartbeat/last run/failures | scheduled work health |
| Storage | capacity/availability/upload failures | file service health |
| Application | sanitized exception/module/version | diagnostics |
| Security/audit | actor/action/entity/time/context per policy | investigation/compliance; restricted access |
| Backup | last successful run/integrity/restore evidence | recovery readiness |

## Security and Retention

Never log password, token, secret, unnecessary personal/payment data or full sensitive payload. Restrict log access, encrypt/transfer safely according to approved policy, rotate and retain based on TBD schedules; audit access to audit/security logs. Correlation IDs connect API/job/integration processing without disclosing record data.

## Alerting and Operations

Create alert on health failures, repeated 5xx, queue backlog/failures, disk pressure, scheduler/worker down and missed/failed backup. Threshold, duration, destination, escalation and runbook owner must be approved; no numerical SLO is invented. Dashboard and notification channels TBD.

## Open Issues

Metrics/logging stack, format, sampling, trace correlation, SLO/thresholds, log retention, privacy review, audit immutability, alert routing/on-call, timezone and access policy.

## References / Change History

MTS §§5.7, 10.3, 15–17; DOC-027–029, DOC-033/036.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Observability signals, redaction and unresolved thresholds | Documentation Agent |
