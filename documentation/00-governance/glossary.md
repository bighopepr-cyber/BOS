# Glossary

| Atribut | Nilai |
|---|---|
| Document Code | GOV-003 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Documentation Manager |
| Related Documents | `documentation/00-governance/document-master-register.md`, `documentation/00-governance/documentation-standard.md`, `docs/MTS.md` |

## Tujuan dan Cakupan

Glosarium menetapkan makna istilah lintas dokumen berdasarkan penggunaan pada MTS. Definisi yang belum ditetapkan MTS ditandai terbuka dan tidak dimaksudkan sebagai kebijakan bisnis.

## Istilah

| Istilah | Definisi |
|---|---|
| API-first | Pendekatan MTS bahwa fungsi bisnis utama tersedia melalui API terstruktur yang juga digunakan frontend dan integrasi berotorisasi. |
| Approval | Keputusan persetujuan oleh approver dalam alur yang dikonfigurasi; aturan kewenangan rinci TBD. |
| Audit log | Catatan aktivitas penting yang sekurangnya memuat actor, action, entity, timestamp, request ID, dan konteks relevan; rahasia tidak dicatat. |
| Business unit | Unit organisasi internal; bentuk, hubungan, dan kebutuhan operasional rinci mengikuti konfigurasi organisasi. |
| BOS (Business Operating System) | Sistem terintegrasi untuk mengelola proses organisasi melalui satu aplikasi. |
| Branch | Cabang dalam struktur organisasi internal; hierarki dan aturan scope rinci mengikuti konfigurasi. |
| Cache | Penyimpanan sementara yang menurut MTS dapat menggunakan Redis. |
| Chart of Accounts (CoA) | Daftar akun yang digunakan pencatatan akuntansi; kebijakan akuntansi rinci TBD. |
| Data scope | Batas record yang boleh diakses pengguna, misalnya scope organisasi, business unit, cabang, departemen, record sendiri, atau proyek yang ditugaskan. |
| Domain | Batas tanggung jawab dan aturan bisnis suatu modul. |
| Domain event | Fakta bahwa sesuatu telah terjadi di domain, digunakan untuk komunikasi internal asynchronous atau integrasi sesuai kontrak. |
| Double-entry accounting | Model pembukuan yang mensyaratkan total debit sama dengan total kredit pada journal yang diposting. |
| Employee | Entitas data kepegawaian; berbeda dari user dan dapat dihubungkan dengan user. |
| Entity | Objek data/domain yang memiliki identitas dan aturan hidup tertentu; model final per modul TBD. |
| ERP | Enterprise Resource Planning; kelas sistem yang menjadi salah satu konteks kemampuan BOS, bukan nama arsitektur BOS. |
| Eventual/asynchronous processing | Pemrosesan lanjutan melalui queue/event setelah request/transaksi awal; kebutuhan konsistensi dan retry ditentukan per proses. |
| Idempotency | Sifat operasi berulang dengan kunci yang sama tidak menggandakan efek transaksi; diwajibkan dipertimbangkan pada operasi berisiko duplikasi. |
| Identity | Data akun autentikasi pengguna. MTS membedakan User dari Employee. |
| Integration contract | Antarmuka yang disepakati untuk interaksi antarmodul atau dengan sistem eksternal; detail tiap integrasi TBD sampai dispesifikasikan. |
| Laravel | Framework backend PHP yang disebut sebagai arah teknologi MTS; versi final mengikuti kompatibilitas saat implementasi. |
| Modular monolith | Satu aplikasi backend dengan modul domain terpisah secara logis dan batas kontrak, bukan microservices. |
| Multi-tenant | Model satu platform melayani banyak pelanggan terisolasi; secara eksplisit di luar cakupan baseline BOS. |
| Organization | Organisasi yang menggunakan satu instalasi; instalasi bersifat single-tenant namun mendukung unit organisasi internal. |
| Outbox / transactional outbox | Pola mencatat event dalam transaksi database agar publikasi perubahan penting tidak kehilangan sinkronisasi; pemilihan implementasi rinci TBD. |
| Permission | Izin aksi yang dapat dirumuskan sebagai `module.resource.action`; definisi katalog final per modul TBD. |
| POS (Point of Sale) | Modul transaksi penjualan langsung dengan terminal, cashier shift, pembayaran, receipt, pengembalian, refund, dan cash closing. |
| PostgreSQL | Database relasional utama yang ditentukan MTS. |
| RBAC (Role-Based Access Control) | Kontrol akses berbasis role yang mengumpulkan permission; dilengkapi pemeriksaan data scope. |
| Redis | Teknologi cache dan queue yang ditetapkan/diajukan dalam baseline MTS; pemakaian operasional rinci perlu spesifikasi. |
| REST API | Gaya API HTTP berbasis resource; prefix baseline MTS `/api/v1`. |
| S3-compatible storage | Object storage dengan antarmuka kompatibel S3; pilihan provider tidak ditetapkan. |
| Single-tenant | Satu instalasi digunakan satu organisasi/lingkungan operasional, bukan satu layanan multi-customer. |
| Soft delete | Penandaan penghapusan logis tanpa menghapus fisik; hanya diterapkan jika kebutuhan domain membenarkan. |
| Stock movement | Catatan transaksi sumber kebenaran perubahan kuantitas stok; saldo tidak boleh diubah tanpa pergerakan yang dapat ditelusuri. |
| TBD (To Be Determined) | Keputusan atau detail belum ditentukan. |
| TBC (To Be Confirmed) | Informasi/proposal menunggu konfirmasi pihak berwenang. |
| ULID | Kandidat strategi identifier yang disebut dalam keputusan terbuka MTS; belum dipilih. |
| UUID | Kandidat strategi identifier yang disebut dalam keputusan terbuka MTS; belum dipilih. |
| User | Identitas untuk autentikasi dan akses aplikasi; bukan sinonim Employee. |
| Workflow | Definisi dan instans alur persetujuan/proses yang dapat memiliki step, penugas approver, routing kondisional, delegasi, revisi, eskalasi, dan riwayat. |

## Singkatan

| Singkatan | Kepanjangan |
|---|---|
| ADR | Architecture Decision Record |
| API | Application Programming Interface |
| AR / AP | Accounts Receivable / Accounts Payable |
| BRS | Business Requirements Specification; pada konteks domain MTS juga dapat berarti Business Rules Specification jika diberi label BRS-RULES |
| CRM | Customer Relationship Management |
| CSV / XLSX | Comma-Separated Values / format spreadsheet Excel Open XML |
| ERD | Entity Relationship Diagram |
| FRS | Functional Requirements Specification |
| HRM / HRIS | Human Resource Management / Human Resource Information System |
| HTTP | Hypertext Transfer Protocol |
| IDOR | Insecure Direct Object Reference |
| JSON | JavaScript Object Notation |
| KPI | Key Performance Indicator |
| LMS | Learning Management System |
| MTS | Master Technical Specification |
| NGINX | Web server/reverse proxy yang disebut dalam MTS |
| POS | Point of Sale |
| RPO / RTO | Recovery Point Objective / Recovery Time Objective; target belum ditetapkan |
| SRS | Software Requirements Specification |
| TBD / TBC | To Be Determined / To Be Confirmed |

## Batasan dan Isu Terbuka

- “Organization”, “company” akuntansi, dan “business unit” tidak boleh dianggap identik; MTS mencatat multi-company accounting sebagai keputusan terbuka.
- Istilah biaya persediaan, payroll, tax, periode akuntansi, retensi, dan SLA memerlukan definisi kebijakan oleh pemilik yang berwenang.
- Glosarium diperbarui ketika dokumen turunan menetapkan istilah tambahan tanpa bertentangan dengan MTS.

## References

- `docs/MTS.md`, v1.0 Draft Baseline.
- `AGENT.md`, standar terminologi dan konsistensi.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Glosarium awal berdasarkan MTS | Documentation Agent |
