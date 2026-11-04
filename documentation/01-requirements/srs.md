# Software Requirements Specification (SRS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-002 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — System/Product Owner |
| Related Documents | `brs.md`, `frs.md`, `../00-governance/glossary.md`, `../../docs/MTS.md` |

## 1. Tujuan dan Cakupan

SRS menetapkan kebutuhan sistem tingkat tinggi yang diturunkan dari BRS dan MTS untuk BOS web self-hosted, single-tenant, modular monolith, API-first. Detail implementasi yang belum diputuskan tidak dianggap final.

## 2. Konteks Sistem

Pengguna mengakses web frontend Next.js/React/TypeScript melalui NGINX ke backend Laravel dan REST API `/api/v1`. Backend memakai PostgreSQL, Redis untuk cache/queue, serta local atau S3-compatible file storage. Docker Compose dan Linux adalah arah deployment MTS. External provider spesifik TBD.

## 3. Persyaratan Sistem

### 3.1 Fungsional

| ID | Persyaratan | Trace ke BRS | Modul |
|---|---|---|---|
| SYS-ORG-001 | Sistem harus menyimpan profil organisasi dan struktur unit internal yang bersifat parent-child; level yang tidak digunakan organisasi tidak wajib dibuat. | BUS-ORG-001 | Core / Organization |
| SYS-ID-001 | Sistem harus menyediakan akun user, login/logout, pengelolaan password, reset, aktivasi/suspensi, profile, dan session management. | BUS-CORE-001 | Core / Identity |
| SYS-AUTH-001 | Backend harus memeriksa autentikasi dan permission sebelum menjalankan aksi terlindungi. | BUS-ACCESS-001 | Core / Access Control |
| SYS-AUTH-002 | Otorisasi harus dapat menggabungkan permission, data scope, akses record, dan kondisi business state. | BUS-ACCESS-001 | Core / Access Control |
| SYS-WF-001 | Sistem harus mendukung definisi/version workflow, step/approver assignment, approval, rejection, revision, delegation, escalation, dan history sesuai konfigurasi. | BUS-WORK-001 | Core / Workflow |
| SYS-NOTIF-001 | Sistem harus mendukung in-app notification, email, dan webhook notification dengan preference/status/retry sesuai channel yang diaktifkan. | BUS-CORE-001 | Core / Notification |
| SYS-AUD-001 | Sistem harus mencatat aktivitas penting dengan actor/action/entity/time/request context dan mengecualikan password, token, serta secret. | BUS-AUD-001 | Core / Audit |
| SYS-DOC-001 | Sistem harus mengelola file di luar public web root dan mengotorisasi akses file sensitif atau memakai signed URL terbatas. | BUS-DATA-001 | Core / Documents |
| SYS-NUM-001 | Sistem harus menghasilkan nomor dokumen tanpa duplikasi pada request bersamaan, berdasarkan konfigurasi sequence yang ditetapkan. | BUS-DATA-001 | Core / Numbering |
| SYS-CF-001 | Sistem dapat menyediakan custom fields terkonfigurasi, tetapi tidak menggantikan atribut inti untuk relasi/integritas/query kritis. | BUS-DATA-001 | Core |
| SYS-IMP-001 | Sistem harus mendukung alur import dengan template/mapping, validasi, preview, laporan error, duplicate detection, dan transaksi yang ditentukan. | BUS-CORE-001 | Core |
| SYS-FIN-001 | Modul Finance harus mencegah posting journal tidak seimbang dan perubahan langsung atas journal posted. | BUS-FIN-001 | Finance |
| SYS-INV-001 | Setiap perubahan persediaan harus mempunyai stock movement yang dapat ditelusuri; transfer mendukung proses pengiriman dan penerimaan. | BUS-INV-001 | Inventory |
| SYS-POS-001 | Transaksi POS berisiko duplikasi harus mendukung penanganan idempotent dan memiliki jejak hasil transaksi/pembayaran. | BUS-POS-001 | POS |
| SYS-REP-001 | Daftar data besar harus dipaginasi dan laporan berat dapat dijalankan asynchronous. | BUS-REPORT-001 | Analytics / all modules |
| SYS-OPS-001 | Sistem harus mengekspos health/operational status yang relevan untuk database, queue, storage, worker, scheduler, error, dan backup. | BUS-OPS-001 | System Administration |
| SYS-INT-001 | Modul harus berinteraksi melalui kontrak public/application service/query/event, bukan menulis tabel internal modul lain secara sembarang. | BUS-DATA-001, BUS-INTEG-001 | Cross-module |

### 3.2 Antarmuka dan Data

| ID | Persyaratan |
|---|---|
| SYS-API-001 | API bisnis utama menggunakan HTTP JSON UTF-8 dengan base path `/api/v1`, format respons konsisten, dan kode HTTP yang sesuai. |
| SYS-API-002 | Collection endpoint harus mendukung pagination; filter/sort hanya sesuai field yang didefinisikan tiap endpoint. |
| SYS-API-003 | Perubahan API yang breaking harus memakai versi baru atau mekanisme migrasi terdokumentasi. |
| SYS-DATA-001 | PostgreSQL adalah database utama; relasi penting memakai FK/constraint dan perubahan skema direpresentasikan migration. |
| SYS-DATA-002 | Primary-key strategy harus konsisten tetapi pilihan UUID/ULID/strategi lain tetap TBD sampai keputusan dibuat. |
| SYS-QUEUE-001 | Job asynchronous mempunyai retry, timeout, logging, dan failure handling yang dispesifikasikan. |

### 3.3 Non-Functional Requirements

| ID | Persyaratan | Target/batas |
|---|---|---|
| SYS-NFR-PERF-001 | API read sederhana p95 | <500 ms pada lingkungan uji yang ditetapkan; target MTS awal, belum jaminan |
| SYS-NFR-PERF-002 | API write sederhana p95 | <800 ms pada lingkungan uji yang ditetapkan; target MTS awal |
| SYS-NFR-PERF-003 | Initial dashboard | <3 detik untuk data/infrastruktur uji yang ditetapkan; target awal |
| SYS-NFR-PERF-004 | Laporan besar | Diproses asynchronous; ambang “besar” TBD |
| SYS-NFR-SEC-001 | Backend menjadi otoritas authorization; HTTPS wajib untuk produksi; secret tidak disimpan di source code. | Kriteria keamanan MTS |
| SYS-NFR-SEC-002 | Sistem melindungi dari SQL injection, XSS, CSRF, broken access control/IDOR, file upload abuse, session attack, dan sensitive data exposure. | Verifikasi DOC-027/030 |
| SYS-NFR-REL-001 | Sistem menyediakan health check, restart policy, backup, queue monitoring, logging, dan restore procedure. | Target availability/RPO/RTO TBD |
| SYS-NFR-MAINT-001 | Sistem mengikuti modular code, automated tests, migration history, API documentation, review, dan dependency management. | MTS §17.4 |
| SYS-NFR-COMP-001 | Matriks kompatibilitas browser/OS/runtime/database/cache harus dipelihara dan versi dependency dikunci saat implementasi. | Versi final TBD |

## 4. Constraint

- Tidak SaaS multi-tenant dan tidak microservices sebagai arsitektur awal.
- PostgreSQL relational integrity; JSON bukan pengganti relasi utama.
- Tidak menetapkan UUID/ULID, session/token strategy universal, multi-currency, inventory costing, payroll scope, real-time messaging, storage provider, search, reporting architecture, offline POS, target kapasitas, RPO/RTO, atau versi final runtime tanpa keputusan.

## 5. Asumsi dan Open Issues

Belum ada asumsi teknis tambahan yang diperlakukan sebagai keputusan. Rincian notifikasi/provider eksternal, rule matrix setiap endpoint, klasifikasi/retensi data, NFR kuantitatif, browser/runtime matrix, dan acceptance thresholds memerlukan spesifikasi lanjutan serta persetujuan pemilik.

| ID | Isu | Status |
|---|---|---|
| SRS-OPEN-001 | Target volume/concurrency dan hardware benchmark belum ditentukan. | TBD |
| SRS-OPEN-002 | Skema authentication client masa depan belum dipilih (cookie session/token). | ADR Required |
| SRS-OPEN-003 | Data classification/retention dan yurisdiksi belum ditentukan. | TBC |
| SRS-OPEN-004 | Kontrak dan SLA provider eksternal belum tersedia. | TBD |

## 6. References

- `brs.md`, DOC-001.
- `frs.md`, DOC-003.
- `../../docs/MTS.md`, khususnya §§1–18, 22–23.
- `../00-governance/documentation-standard.md`.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | SRS awal diturunkan dari MTS dan BRS | Documentation Agent |
