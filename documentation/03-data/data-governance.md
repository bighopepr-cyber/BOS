# Data Governance Specification (DGS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-016 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Data Governance Owner |
| Related Documents | `database-design.md`, `data-dictionary.md`, `data-migration.md`, `../06-security-infrastructure/security-specification.md`, `../../docs/MTS.md` |

## 1. Tujuan

Menetapkan governance konseptual atas kepemilikan, akses, kualitas, lifecycle dan perlindungan data. MTS belum menentukan klasifikasi resmi, retensi, yurisdiksi atau privacy policy; tabel ini adalah kerangka rekomendasi yang memerlukan persetujuan.

## 2. Prinsip yang Ditentukan MTS

- Satu database installation untuk satu organisasi; struktur internal organisasi digunakan untuk scope data.
- Tiap modul memiliki ownership domain; modul lain mengakses data melalui kontrak.
- PostgreSQL relational integrity, migration, transaction, audit trail untuk transaksi penting.
- File sensitif tidak public; backup dilindungi; secret/credential tidak dicatat di logs.
- Historical transaction tidak boleh rusak akibat penghapusan master; financial data tidak dihapus secara fisik melalui proses normal.

## 3. Ownership dan Access

| Data domain | System of record | Access governance |
|---|---|---|
| Organization hierarchy | Organization | RBAC + organizational data scope |
| User/credential | Identity | Least privilege; credential hash; session controls |
| Employee | HRM | Scope department/branch/own record sesuai permission; field classification TBD |
| Finance ledger | Finance | Permission, period state, audit; posted entry correction via reversal/adjustment |
| Stock | Inventory | Movement-backed, warehouse/location scope TBD |
| CRM/Sales/POS | Domain ownership perlu dikonfirmasi | Customer/product access contracts TBD |
| Documents/files | Documents module dengan domain owner pada relation | Authorization atau signed URL terbatas |
| Audit/security logs | Audit/System | Read access dibatasi; redaction dan retention TBD |

## 4. Klasifikasi dan Lifecycle (Kerangka Proposed)

Klasifikasi yang dapat dipakai setelah disetujui: public, internal, confidential, restricted. Data pribadi employee, credential, payroll, financial, payment, document, audit dan secrets perlu klasifikasi pemilik sebelum implementasi. Retention period, archival, deletion/legal hold, data subject handling, residency, encryption-at-rest and key custody semuanya TBD; tidak diasumsikan regulasi tertentu.

Lifecycle minimal dicatat per data set: create/source, validate, use/share, archive, retain, dispose; owner menetapkan lifecycle dan approval.

## 5. Quality, Lineage, and Reporting

- Entity master memiliki steward/source of truth, validation, duplicate policy dan change history.
- Transactional data menyimpan source reference/correlation ID bila relevan.
- Derived balances/reports menyatakan freshness, calculation ownership, reconciliation, and rebuild method.
- Analytics reads are authorized and controlled; direct unrestricted joins are not assumed.
- Data export records user, scope, fields, time and purpose where policy requires; detailed audit policy TBD.

## 6. Backup and Security

Backup database/files/configuration and necessary keys follows protected access/retention; restore tests verify integrity. Secret material must be stored separately from source code and protected by operational controls. Encryption at rest, key rotation, backup retention and destruction require security/operations decisions.

## 7. Open Issues

| ID | Isu | Status/owner |
|---|---|---|
| DGS-OPEN-001 | Data classification taxonomy, data owners/stewards and approval process | TBC — Business/Security |
| DGS-OPEN-002 | Retention, archival, deletion, privacy/legal hold and jurisdiction | TBD — Legal/Business |
| DGS-OPEN-003 | Encryption at rest/key management and secret rotation | ADR Required — Security/Ops |
| DGS-OPEN-004 | Customer/product master ownership across CRM/Sales/POS/Inventory | TBC — Domain Owners |
| DGS-OPEN-005 | Data quality thresholds, duplicate resolution and export audit requirements | TBD — Data Owner |

## References / Change History

MTS §§2.1, 2.5, 5.7, 7.3/7.8, 10–11, 15; DOC-013/015, DOC-027/029.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Prinsip governance dan isu yang perlu keputusan pemilik data | Documentation Agent |
