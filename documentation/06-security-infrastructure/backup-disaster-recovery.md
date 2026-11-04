# Backup & Disaster Recovery Specification (BDRS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-029 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Operations/Data Owner |
| Related Documents | `infrastructure-deployment.md`, `../03-data/data-governance.md`, `../09-operations/deployment-manual.md`, `../../docs/MTS.md` |

## 1. Tujuan dan Scope

Melindungi dan memulihkan data PostgreSQL, uploaded files, application configuration dan necessary encryption keys/secrets melalui prosedur penyimpanan aman. MTS requires documented restore and periodic test. Backup schedules, destination, retention, RPO/RTO and encryption/key procedure belum diputuskan.

## 2. Backup Scope/Types

- PostgreSQL: full backup; incremental/PITR hanya jika tool/operasi dipilih dan diuji.
- Files/object storage: konsisten dengan backup database dan version metadata.
- Configuration/secrets: separate restricted secure backup; jangan menyimpan kunci bersama backup terenkripsi dengan akses yang sama tanpa kontrol.
- Include application version/schema migration metadata; exclude reproducible artifacts only after restore test proves safe.

## 3. Controls

Least-privilege backup/restore access, encryption in transit/at rest as selected, integrity checks, immutable/off-site copy decision, inventory and timestamps, retention/deletion policy, alert on failed/late backup, audit access, and credentials/key rotation. Do not write secrets to application logs. Media failure/ransomware threat model and off-site topology TBD.

## 4. Restore Procedure (High Level)

1. Declare recovery event and authorize restore; identify target recovery point.
2. Isolate environment and verify compatible application/runtime/schema version.
3. Retrieve backup and keys via separate authorized paths; validate checksums.
4. Restore PostgreSQL and files/configuration to clean target; preserve original for investigation when required.
5. Run DB integrity/FK checks, file-reference checks, business reconciliations (Finance journals; Inventory balances), auth/security checks, and health tests.
6. Reconcile changes after recovery point; obtain owner signoff before reconnecting users/integrations.
7. Record recovery timeline, data loss, actions and lessons learned; rotate potentially exposed secrets.

Exact commands/tooling belong in deployment manual after stack selection; no destructive procedure is prescribed here.

## 5. RPO/RTO and Test

RPO/RTO not defined by MTS and must be approved by business/operations. Define backup frequency, retention, recovery scope, restore time budget and test cadence from those objectives. At minimum perform periodic restore tests in isolated environment and prove application starts, data reconciles and uploaded files are readable. Evidence and pass/fail criteria TBD.

## 6. Open Issues

Backup tool, frequency, retention, point-in-time recovery, destination/offsite, encryption/key custody, data consistency, restore priority, RPO/RTO, test cadence, owners/on-call, integrity and acceptance thresholds.

## References / Change History

MTS §§10.2, 15.5, 16.1, 16.4, 17.3; DOC-016/027/028/032/038.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Backup scope, security controls and restore framework | Documentation Agent |
