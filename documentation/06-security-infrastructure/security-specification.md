# Security Technical Specification (STS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-027 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Security Owner |
| Related Documents | `../04-backend/authentication-authorization.md`, `../02-architecture/sad.md`, `infrastructure-deployment.md`, `backup-disaster-recovery.md`, `../../docs/MTS.md` |

## 1. Tujuan dan Threat Boundary

Merinci baseline security by design untuk self-hosted single-tenant modular monolith. Ancaman yang disebut MTS: SQL injection, XSS, CSRF, broken access control/IDOR, file upload abuse, authentication attacks, session hijacking, mass assignment dan sensitive data exposure. Threat model, classification, regulator dan risk appetite perlu owner review.

## 2. Security Controls

| Area | Kontrol baseline |
|---|---|
| Authentication | Password hash framework recommended, reset/session revocation/throttle; MFA optional; strategy final TBD. |
| Authorization | Backend-only authority; least privilege, deny by default, RBAC + scope + record + state; test IDOR/list/detail/export/download. |
| Input/data | Server validation, parameterized ORM/query, mass-assignment allowlist, output encoding, CSRF/session controls. |
| API | HTTPS, rate limiting, appropriate status/error, no stack traces/secrets, idempotency for risk operations. |
| Upload | Outside public root, type/size/content validation, random storage name, authorization on access; scanning/limits TBD. |
| Secrets/crypto | No secrets in code/logs; HTTPS production; storage/at-rest encryption and key management need decision. |
| Audit/log | Critical action trace, restricted access, redact password/token/secret and sensitive payload; retention TBD. |
| Database | Least-privilege app credentials, FK/constraints, network isolation and backup access restrictions; specifics DOC-028/029. |
| Dependencies | Version compatibility lock, update/patch/vulnerability review process. |
| Admin | Separate privileged permissions, access review, session review and update/backup operation trail. |

## 3. Secure Development and Verification

Threat review for authentication, permissions, files, finance/POS/inventory, import/export, webhook and admin paths. Automated/manual checks: SQLi, XSS, CSRF, IDOR, privilege escalation, upload abuse, session handling, rate limits, secrets/log leakage, dependency scanning and backup access. Severity definitions, acceptance thresholds, penetration-test scope and incident disclosure process TBD.

## 4. Operations

Security patching, access review, log review, backup verification, incident response and vulnerability review are required activities per MTS. Assign owners, cadence, alert SLA, contact tree and evidence retention before production.

## 5. Open Issues

Data classification/regulatory scope, encryption-at-rest and keys, MFA/password policy, secret manager, network/firewall/ports, WAF requirement, malware scanning, security headers/CSP, session durations, vulnerability SLAs and incident response RACI.

## References / Change History

MTS §§2.5, 5.2–5.7, 9–10, 15, 16, 18; DOC-021/028/029/030.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Security baseline controls and unresolved policy decisions | Documentation Agent |
