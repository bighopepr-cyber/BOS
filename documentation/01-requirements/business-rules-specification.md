# Business Rules Specification (BRS-RULES)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-006 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Business Rule Owners |
| Related Documents | `brs.md`, `frs.md`, `use-case-specification.md`, `business-process-specification.md`, `../../docs/MTS.md` |

## 1. Tujuan dan Cakupan

Mengkonsolidasikan aturan bisnis eksplisit dari MTS dan memisahkannya dari pilihan yang masih terbuka. Kode aturan dapat ditautkan ke requirement, workflow, implementasi, dan test. Dokumen ini bukan pengganti kebijakan Finance/HR/Inventory yang perlu persetujuan pemilik.

## 2. Aturan yang Ditetapkan MTS

| ID | Aturan | Pemilik domain | Trace / validasi |
|---|---|---|---|
| RULE-CORE-001 | Satu instalasi digunakan satu organisasi; tidak diperlukan tenant ID untuk isolasi antarpelanggan, tenant resolver, provisioning, atau tenant routing middleware. | Core | BUS-CORE-002; SYS-ORG-001 |
| RULE-CORE-002 | User dan Employee adalah konsep/entitas berbeda; hubungan antara keduanya dapat dibuat. | Core/HRM | FUN-ID-001; UC-CORE-001 |
| RULE-CORE-003 | Role mengelompokkan permission; otorisasi juga memeriksa data scope, record, dan business state. | Core | FUN-AUTH-001/002; UC-CORE-002 |
| RULE-CORE-004 | Backend adalah otoritas permission; pemeriksaan UI tidak cukup untuk mengizinkan aksi. | Core/Security | SYS-AUTH-001; security tests DOC-030 |
| RULE-CORE-005 | Aktivitas sensitif dapat diaudit; password, token, dan secret tidak boleh direkam dalam audit log. | Core | FUN-AUD-001 |
| RULE-CORE-006 | File disimpan di luar public web root; akses file sensitif melalui authorization check atau signed URL berumur terbatas. | Core | SYS-DOC-001 |
| RULE-CORE-007 | Sequence nomor dokumen harus aman dari concurrent request dan tidak menghasilkan nomor ganda. | Core | FUN-NUM-001; concurrency test |
| RULE-FIN-001 | Setiap journal yang diposting harus balance: total debit sama dengan total kredit. | Finance | FUN-FIN-001; UC-FIN-001 |
| RULE-FIN-002 | Journal posted tidak boleh diedit langsung; koreksi melalui reversal atau adjustment. | Finance | FUN-FIN-001; BPS-FIN-001 |
| RULE-FIN-003 | Periode akuntansi dapat dikunci; transaksi harus memiliki referensi sumber dan audit trail. | Finance | FUN-FIN-001; detail locking TBD |
| RULE-FIN-004 | Nilai uang menggunakan tipe numerik presisi, bukan floating point. | Finance | DDS; precision/scale TBD |
| RULE-INV-001 | Semua perubahan stok harus menghasilkan stock movement; saldo tidak boleh diubah tanpa transaksi terlacak. | Inventory | FUN-INV-001/002; UC-INV-001 |
| RULE-INV-002 | Transfer antar gudang memiliki proses pengiriman dan penerimaan. | Inventory | FUN-INV-001; BPS-INV-001 |
| RULE-INV-003 | Kebijakan costing persediaan harus ditetapkan eksplisit sebelum implementasi valuasi. | Inventory/Finance | TBD; ADR/business decision required |
| RULE-POS-001 | Operasi POS yang berisiko duplikasi, terutama pembayaran dan pencatatan stok, harus dilindungi dari request duplikat. | POS | FUN-POS-001; idempotency policy TBD |
| RULE-DATA-001 | Foreign key/unique constraint/index diterapkan sesuai kebutuhan integritas dan pola query; penghapusan master tidak boleh merusak transaksi historis. | Data | DDS/ERS |
| RULE-MOD-001 | Modul tidak mengakses/menulis tabel internal modul lain secara sembarang; interaksi memakai kontrak yang disepakati. | Architecture | SYS-INT-001; DOC-010–012 |
| RULE-API-001 | API collection menggunakan pagination; breaking changes memerlukan versi baru atau migrasi terdokumentasi. | API | SYS-API-002/003 |

## 3. Aturan Konfigurasi dan Alur

- Konfigurasi operasional tidak menggantikan business logic yang memerlukan validasi/integrity controls.
- Role bawaan dapat dikonfigurasi.
- Workflow memiliki history yang tidak dapat diubah sembarangan.
- Reporting harus menggunakan query terkontrol; pekerjaan berat dapat asynchronous atau memakai read model khusus.
- Soft delete tidak diterapkan ke semua tabel secara otomatis; keputusan dilakukan per kebutuhan domain.

## 4. Keputusan Bisnis/Teknis yang Belum Ditentukan

| ID | Aturan/keputusan yang dibutuhkan | Status |
|---|---|---|
| RULE-OPEN-001 | UUID vs ULID/strategi primary key | ADR Required |
| RULE-OPEN-002 | Multi-currency, currency precision, exchange-rate source, rounding policy | TBD |
| RULE-OPEN-003 | Accounting calendar/fiscal year, close/reopen, toleransi rounding, dimensions, approval thresholds | TBC |
| RULE-OPEN-004 | Inventory costing (FIFO/weighted average/opsi lain), negative stock, reservation, discrepancy handling | ADR Required |
| RULE-OPEN-005 | Payroll scope, formula, period, legal/tax jurisdiction, integration ke Finance | TBC |
| RULE-OPEN-006 | Tax rules dan price/discount authorization untuk Sales/POS | TBC |
| RULE-OPEN-007 | POS payment timeout, offline mode, return/refund, shift cash discrepancy | TBC |
| RULE-OPEN-008 | Approval matrix, delegation, escalation schedule, workflow branching, history retention | TBC |
| RULE-OPEN-009 | Perusahaan akuntansi vs Organization/business unit; kebutuhan multi-company | ADR Required |
| RULE-OPEN-010 | Data retention, archival, deletion/privacy obligations, backup RPO/RTO | TBD |

## 5. Kontrol Implementasi dan Verifikasi

Setiap rule yang diimplementasikan harus memiliki validasi di backend/domain, transaksi/constraint yang sesuai, error yang dapat dikenali tanpa membocorkan data, audit bila kritis, dan test case. Rule TBD tidak boleh dikodekan sebagai default bisnis tanpa persetujuan. Traceability detail akan diperluas pada GOV-005 dan DOC-030.

## 6. References

- `../../docs/MTS.md`, §§2, 5–11, 17–18, 22.
- `frs.md` DOC-003; `business-process-specification.md` DOC-005.
- `../00-governance/documentation-standard.md`.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Aturan eksplisit MTS dipisahkan dari keputusan bisnis terbuka | Documentation Agent |
