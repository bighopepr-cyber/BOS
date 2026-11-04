# Documentation Plan & Dependencies

| Atribut | Nilai |
|---|---|
| Document Code | GOV-004 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Documentation Manager |
| Related Documents | `documentation/00-governance/document-master-register.md`, `documentation/00-governance/documentation-standard.md`, `docs/MTS.md`, `AGENT.md` |

## 1. Tujuan

Menetapkan urutan penyusunan DOC-001–DOC-040 dan document pack modul, prasyarat, serta gerbang review. Rencana mengikuti fase pada `AGENT.md` bagian 15 dan dapat disesuaikan hanya dengan mencatat dampak dependensinya.

## 2. Aturan Eksekusi

- Gunakan MTS v1.0 Draft Baseline dan `AGENT.md` sebagai sumber acuan.
- Selesaikan fondasi governance terlebih dahulu; jangan membuat dokumen hilir sebelum prasyaratnya tersedia.
- Setelah setiap dokumen, validasi konsistensi MTS, requirement ID, asumsi/TBD, dependency, diagram, dan referensi; update register dan traceability.
- Pertahankan status `DRAFT` sampai review manusia. Catat pilihan baru sebagai ADR Required, bukan keputusan tersirat.
- Pack modul mengikuti 19 topik minimum dan mengacu aturan global untuk menghindari duplikasi.

## 3. Dependency Plan

| Urutan | Fase / deliverable | Prasyarat | Gerbang keluar |
|---|---|---|---|
| 0 | Analisis baseline; GOV-001 register, GOV-002 standard, GOV-003 glossary, GOV-004 plan | Baca seluruh MTS | Inventaris modul, baseline/proposed, open decisions dan konflik tercatat |
| 1.1 | DOC-001 BRS | Fase 0 | Kebutuhan bisnis berbasis sumber, tanpa kebijakan rekaan |
| 1.2 | DOC-002 SRS | DOC-001 | Persyaratan sistem fungsional/nonfungsional teridentifikasi |
| 1.3 | DOC-003 FRS | DOC-001–002 | Requirement fungsional ber-ID dan pemilik modul |
| 1.4 | DOC-004 UCS | DOC-002–003 | Aktor, alur utama/alternatif, prasyarat, hasil, kontrol akses |
| 1.5 | DOC-005 BPS | DOC-001, DOC-003 | Alur proses lintas peran/modul dan titik keputusan terdokumentasi |
| 1.6 | DOC-006 BRS-RULES | DOC-003, DOC-005 | Aturan domain terpisah dari asumsi dan ditautkan ke test |
| 2.1 | DOC-007 SAD | MTS, DOC-002 | Konteks/lapisan/batas arsitektur konsisten dengan baseline |
| 2.2 | DOC-008 SAS | DOC-007 | Komponen dan deployment/system interactions terperinci |
| 2.3 | DOC-009 ADR | MTS, DOC-007 | Baseline/proposed dan opsi terbuka dipertahankan; persetujuan tercatat |
| 2.4 | DOC-010 MAS | DOC-007, inventory modul | Kontrak, ownership, dan batas tiap modul jelas |
| 2.5 | DOC-011 IAS | DOC-007, DOC-010 | Integrasi internal/eksternal, sinkron/asinkron, failure handling jelas |
| 2.6 | DOC-012 MDM | DOC-010–011 | Dependency langsung dan aturan arah dependensi ditinjau |
| 3.1 | DOC-013 DDS | DOC-010, DOC-012 | Tipe/PK/FK/constraint/index/ownership dan TBD terdefinisi |
| 3.2 | DOC-014 ERS | DOC-013 | ERD sesuai model dan batas domain |
| 3.3 | DOC-015 DD | DOC-013–014 | Kamus data konsisten dengan relasi dan API yang direncanakan |
| 3.4 | DOC-016 DGS | DOC-001, DOC-013 | Ownership, klasifikasi, akses, retensi, lifecycle; kebijakan belum ada ditandai TBD |
| 3.5 | DOC-017 DMS | DOC-013, DOC-016 | Sumber, mapping, validasi, rekonsiliasi, rollback dan cutover direncanakan |
| 4.1 | DOC-018 BTS | DOC-007, DOC-010, DOC-013 | Struktur backend, transaksi, modul, error dan operasi teknis konsisten |
| 4.2 | DOC-019 API-SPEC | DOC-018, DOC-015, DOC-021 | Endpoint kontraktual lengkap; tidak ada endpoint tanpa fungsi |
| 4.3 | DOC-020 EQS | DOC-011, DOC-018 | Event/job, retry, timeout, idempotensi, observability tercatat |
| 4.4 | DOC-021 AAS | DOC-002, DOC-007 | Authentication, RBAC, scope, session, server-side enforcement ditetapkan |
| 4.5 | DOC-022 WES | DOC-005–006, DOC-018 | State/transition, approver, history, delegation, failure behavior terdefinisi |
| 5.1 | DOC-026 IA | DOC-003, DOC-010 | Navigasi dan struktur informasi berdasar capability/role, bukan asumsi final |
| 5.2 | DOC-023 UIUX | DOC-002–003, DOC-026 | State, aksesibilitas, error/empty/loading, alur utama terdefinisi |
| 5.3 | DOC-025 DSS | DOC-023 | Token/komponen/varian dan standar aksesibilitas terdokumentasi |
| 5.4 | DOC-024 FTS | DOC-007, DOC-023, DOC-025 | Arsitektur frontend dan kontrak data jelas |
| 6.1 | DOC-027 STS (Security) | DOC-007, DOC-021 | Threat/control requirements dan operasi keamanan tercatat |
| 6.2 | DOC-028 IDS | DOC-007, DOC-018, DOC-024 | Topologi, konfigurasi, instalasi, health check, update terdefinisi |
| 6.3 | DOC-029 BDRS | DOC-013, DOC-028 | Backup, restore drill, RPO/RTO TBD bila belum diputuskan |
| 7 | 12 document pack modul | DOC-010–022, DOC-013–015, DOC-023–027 | Tiap pack mencakup 19 topik; aturan lintas modul dan TBD konsisten |
| 8.1 | DOC-030 STS-TEST | DOC-002–003, DOC-019, module packs | Strategi unit/integration/API/E2E/security/performance traceable |
| 8.2 | DOC-031 QAAS, GOV-005/GOV-006 traceability | DOC-001–030 | Acceptance dan test tertaut ke requirement; gap dicatat |
| 9 | DOC-032–040 operasi | DOC-028–031 serta domain terkait | Instruksi operasional dapat dijalankan dan memiliki pemilik/parameter TBD |

Urutan frontend di atas mempertahankan dependensi IA → UI/UX → design system → frontend technical specification, sembari mengacu ke urutan AGENT.md yang mengelompokkan dokumen frontend.

## 4. Urutan Document Pack Modul

| Urutan | Pack | Dependency utama |
|---|---|---|
| 1 | Core System | DOC-010, DOC-013–015, DOC-021–022 |
| 2 | HRM / HRIS | Core; DOC-005–006; kontrak Finance/LMS yang disetujui |
| 3 | Finance & Accounting | Core; DOC-006; aturan double-entry dari MTS; kebijakan akuntansi TBD |
| 4 | CRM | Core; kontrak Sales |
| 5 | Sales Management | Core, CRM, Finance; DOC-011–012 |
| 6 | Inventory Management | Core, Finance; costing TBD |
| 7 | Project Management | Core, HRM, Finance |
| 8 | Point of Sale | Core, Inventory, Finance; offline mode TBD |
| 9 | Collaboration | Core; realtime capability TBD |
| 10 | Learning Management System | Core, HRM |
| 11 | Analytics & Reporting | Kontrak query/data dari modul sumber; reporting architecture TBD |
| 12 | System Administration | DOC-028–029, DOC-032–033, DOC-038 |

Ketergantungan pada tabel ini adalah rencana awal yang diturunkan dari integrasi dan roadmap MTS, bukan keputusan implementasi final. Perubahan dependency dicatat di DOC-012.

## 5. Open Decisions dan Risiko Perencanaan

1. MTS memiliki keputusan Proposed dan keputusan terbuka; perlu review pemilik arsitektur sebelum implementasi.
2. Kebijakan bisnis Finance, inventory costing, payroll/yurisdiksi, pajak, POS/offline, dan multi-company accounting perlu pemilik bisnis terkait.
3. Target pengguna, volume, concurrency, hardware, RPO/RTO, availability, retensi, dan provider eksternal belum tersedia.
4. Detail model organisasi, currency, identifiers, auth strategy, search, dan reporting perlu keputusan/ADR.
5. Tanpa sumber kebutuhan atau pemilik keputusan, spesifikasi hilir hanya dapat mendefinisikan kontrak provisional dan open issue.

## 6. Monitoring Kemajuan

GOV-001 adalah sumber status; dokumen ini menjelaskan urutan. Untuk setiap deliverable, catat status, versi, owner, reviewer/approver (bila ada), dependency yang telah dipenuhi, tanggal review, dan unresolved issues. Perbarui GOV-005/GOV-006 saat requirement/test tersedia.

## References

- `AGENT.md`, workflow bagian 15, struktur bagian 16, validasi bagian 17, eksekusi bagian 18.
- `docs/MTS.md`, khususnya baseline arsitektur, open technical decisions, testing, roadmap, dan architectural decisions.
- `documentation/00-governance/document-master-register.md`.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Rencana lengkap fase, dependency, pack modul, dan gerbang review | Documentation Agent |
