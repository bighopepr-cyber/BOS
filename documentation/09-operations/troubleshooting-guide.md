# Troubleshooting Guide

| Atribut | Nilai |
|---|---|
| Document Code | DOC-036 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Support/Operations Owner |
| Related Documents | `deployment-manual.md`, `system-administration-manual.md`, `monitoring-logging.md`, `../06-security-infrastructure/backup-disaster-recovery.md` |

## Triage Principles

Capture time/timezone, app version, module/operation, sanitized request ID, affected scope, impact and recent change. Do not capture password/token/secret or unapproved production data. Avoid restarting/replaying payments, POS, journals or inventory mutations without idempotency/status verification.

## Symptom Matrix

| Symptom | First checks | Safe next action |
|---|---|---|
| Login failure | Account state, auth service health, rate-limit, clock/network | Avoid repeated attempts; admin checks sanitized auth log and reset procedure |
| 401/403 or missing record | Session, permission, scope, record state | Verify expected role/scope with admin; do not bypass by direct URL |
| API 409/422 | Conflict/current state/validation response | Reload permitted state; correct input; do not blindly retry transaction |
| Queue/report/import pending | Worker health, queue depth, failed job/correlation ID | Check job status and retry policy; operator follows DOC-020 |
| File unavailable | Storage health, reference, authorization/signed URL expiry | Re-authorize and renew access; do not expose public storage URL |
| Finance journal rejected | Balance, account, period, permission/approval | Correct draft; never edit posted journal directly |
| Stock transfer mismatch | Dispatch/receipt movements and source/destination | Stop adjustment; reconcile movement records with Inventory owner |
| Slow API/dashboard | Time window, endpoint, DB/Redis/storage health, workload | Compare to approved benchmark; collect sanitized metrics |
| Backup/restore alert | Backup timestamp, integrity check, destination access | Escalate to authorized operator; do not overwrite source |

## Escalation

Security incident or suspected exposure → security owner; financial/stock discrepancy → domain owner; data loss/restore → operations and business approver; repeated server error → engineering with sanitized correlation ID. Contact matrix, severity and response SLA TBD.

## References / Change History

MTS §§10, 15–18; DOC-027–029, DOC-033, DOC-038.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Initial symptom triage and safety guidance | Documentation Agent |
