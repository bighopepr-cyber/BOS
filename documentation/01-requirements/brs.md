# Business Requirements Specification (BRS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-001 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Business Product Owner |
| Related Documents | `../00-governance/glossary.md`, `../00-governance/documentation-plan.md`, `../../docs/MTS.md` |

## 1. Tujuan

Mencatat kebutuhan bisnis tingkat sistem yang dapat ditelusuri ke MTS. Dokumen ini menjadi masukan bagi SRS dan tidak menetapkan kebijakan operasional yang belum dikonfirmasi.

## 2. Cakupan

BOS self-hosted dan single-tenant untuk satu organisasi per instalasi, dengan struktur internal multi-unit dan fungsi operasional lintas modul. SaaS multi-tenant, tenant billing/provisioning, marketplace, serta microservices sebagai arsitektur awal berada di luar cakupan (MTS §1.4).

## 3. Fakta, Asumsi, dan Keputusan Terbuka

- **Fakta MTS:** satu instalasi melayani satu organisasi; sistem modular monolith API-first; ada core capabilities dan sepuluh business modules.
- **Asumsi sementara:** organisasi membutuhkan pengendalian akses berbasis tanggung jawab dan lingkup data, konsisten dengan MTS RBAC/data scope; model role bisnis rinci belum diketahui.
- **TBD:** ukuran organisasi, yurisdiksi, mata uang, volume/transaksi, jam operasional, SLA/availability, RPO/RTO, serta prioritas rollout.

## 4. Business Requirements

| ID | Kebutuhan bisnis | Sumber | Kriteria hasil tingkat bisnis | Status |
|---|---|---|---|---|
| BUS-CORE-001 | Konsolidasikan operasi organisasi dalam satu aplikasi BOS terintegrasi. | MTS §1.2 | Kapabilitas core dan modul bisnis tersedia dalam satu instalasi dengan batas domain terdokumentasi. | Baseline |
| BUS-CORE-002 | Layani satu organisasi per instalasi tanpa model SaaS multi-tenant. | MTS §1.3, §1.4 | Data pelanggan/tenant lain tidak termasuk model layanan awal; organisasi internal tetap dapat memiliki unit/cabang/divisi. | Baseline |
| BUS-ORG-001 | Representasikan struktur internal organisasi yang dapat dikonfigurasi. | MTS §5.1 | Dapat memodelkan level organisasi yang dibutuhkan tanpa mewajibkan seluruh level konseptual. | Baseline; detail TBD |
| BUS-ACCESS-001 | Batasi akses berdasarkan hak tindakan dan lingkup record internal. | MTS §5.3–5.4 | Keputusan akses mempertimbangkan permission, data scope, dan business state. | Baseline |
| BUS-WORK-001 | Dukung alur persetujuan untuk proses bisnis yang membutuhkannya. | MTS §5.5 | Definisi alur, assignment, hasil, dan history dapat ditelusuri; proses/otoritas rinci TBD. | Baseline; detail TBD |
| BUS-AUD-001 | Dukung audit aktivitas penting dan transaksi yang dapat ditelusuri. | MTS §5.7, §6.2 | Aktivitas kritis memiliki actor, action, waktu, entity, dan konteks relevan tanpa mencatat rahasia. | Baseline |
| BUS-DATA-001 | Jaga integritas dan kepemilikan data antarmodul. | MTS §2.2, §3.3 | Sumber data otoritatif dan kontrak lintas modul ditetapkan, tanpa penulisan langsung yang tidak terkontrol. | Baseline |
| BUS-FIN-001 | Catat transaksi akuntansi secara seimbang dan dapat diaudit. | MTS §6.2, §7.6 | Journal posted balance; koreksi memakai reversal/adjustment; rincian kebijakan accounting TBD. | Baseline |
| BUS-INV-001 | Telusuri perubahan stok melalui transaksi persediaan. | MTS §6.6, §7.7 | Perubahan saldo memiliki stock movement; costing policy ditetapkan sebelum implementasi valuasi. | Baseline |
| BUS-POS-001 | Dukung penjualan langsung beserta penutupan kas dan integrasi. | MTS §6.7 | Siklus transaksi, payment, receipt, return/refund, closing, dan dampak inventory/finance ditentukan. | Baseline; rincian TBD |
| BUS-HRM-001 | Dukung siklus hidup SDM dari data karyawan hingga proses terkait. | MTS §6.1 | Subdomain employee, recruitment, attendance, leave, payroll, performance, training tersedia sesuai prioritas yang disepakati. | Baseline; prioritas TBD |
| BUS-REPORT-001 | Sediakan informasi operasional/manajerial lintas domain. | MTS §6.9 | Dashboard/report memiliki query terkontrol, filter dan ekspor sesuai kebutuhan; KPI dan hak akses TBD. | Baseline; detail TBD |
| BUS-OPS-001 | Operasikan sistem self-hosted dengan prosedur instalasi, pembaruan, pemantauan, backup, dan restore. | MTS §15–16 | Operator dapat mengikuti prosedur terdokumentasi dan memverifikasi kesehatan serta pemulihan. | Baseline; target TBD |
| BUS-INTEG-001 | Izinkan integrasi internal dan eksternal yang terotorisasi melalui kontrak terdokumentasi. | MTS §2.3, §11 | Integrasi menangani otorisasi, kegagalan, retry/idempotency jika relevan. Provider dan use case eksternal TBD. | Baseline |

## 5. Batasan dan Dependensi

Dependensi meliputi persetujuan pemilik bisnis atas prioritas modul, yurisdiksi, akuntansi, payroll, pajak, target operasi, dan integrasi eksternal. Teknologi/arsitektur mengikuti status dalam MTS; keputusan berlabel Proposed belum disahkan.

## 6. Open Issues

| ID | Isu | Pemilik keputusan | Status |
|---|---|---|---|
| BUS-OPEN-001 | Segmentasi organisasi, company akuntansi, dan kebutuhan multi-company belum ditentukan. | Business Owner / Finance Owner | TBC |
| BUS-OPEN-002 | Yurisdiksi, mata uang, pajak, dan kebijakan payroll belum diberikan. | Finance/HR Owner | TBD |
| BUS-OPEN-003 | Sasaran pengguna, volume data, concurrency, availability, RPO/RTO, dan retensi belum ditentukan. | Operations Owner | TBD |
| BUS-OPEN-004 | Prioritas dan urutan rollout modul perlu konfirmasi pemilik produk. | Product Owner | TBC |

## 7. References

- `../../docs/MTS.md`, v1.0 Draft Baseline.
- `../00-governance/documentation-standard.md`.
- `../00-governance/glossary.md`.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Draf kebutuhan bisnis berdasarkan fakta MTS | Documentation Agent |
