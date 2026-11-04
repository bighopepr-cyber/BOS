# Architecture Decision Records (ADR)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-009 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Architecture Review Board |
| Related Documents | `../sad.md`, `../../docs/MTS.md` |

## Tujuan

Mencatat keputusan arsitektur menurut sumber MTS dan melacak keputusan yang masih terbuka. Tabel ini menyalin status sumber, tidak mengubahnya. Tidak ada persetujuan manusia yang dicatat; seluruh dokumen tetap DRAFT.

## Keputusan pada MTS

| ID MTS | Keputusan | Status di MTS | Perlakuan dokumentasi |
|---|---|---|---|
| ADR-001 | Self-hosted deployment | Baseline | Dipakai sebagai constraint |
| ADR-002 | Single-tenant installation | Baseline | Dipakai sebagai constraint |
| ADR-003 | Modular monolith | Baseline | Dipakai sebagai constraint; bukan microservices |
| ADR-004 | Laravel backend | Proposed | Arah stack; persetujuan/status final dibutuhkan |
| ADR-005 | Next.js frontend | Proposed | Arah stack; persetujuan/status final dibutuhkan |
| ADR-006 | PostgreSQL database | Proposed | MTS menyebut sebagai database utama; status tabel Proposed dipertahankan dan perlu klarifikasi |
| ADR-007 | Redis cache and queue | Proposed | Arah teknologi; persetujuan/status final dibutuhkan |
| ADR-008 | REST API | Baseline | API contract `/api/v1` mengikuti MTS |
| ADR-009 | Docker Compose deployment | Proposed | Arah deployment; persetujuan/status final dibutuhkan |
| ADR-010 | RBAC with data scope | Baseline | Backend enforcement wajib |
| ADR-011 | Domain-based module boundaries | Baseline | Kontrak antarmodul wajib |
| ADR-012 | Event-driven internal integration | Baseline | Synchronous API dan asynchronous events sesuai kebutuhan |

## ADR Required / Open Decision Log

| ID | Keputusan | Konteks/opsi yang diketahui | Dampak bila terbuka | Status |
|---|---|---|---|---|
| ADR-OPEN-001 | UUID vs ULID/strategi ID | MTS mencantumkan UUID atau strategi ID konsisten dan menandai pilihan UUID/ULID terbuka | Skema, API, migration, index, integrasi | ADR Required |
| ADR-OPEN-002 | Authentication untuk seluruh client | Cookie session adalah pilihan baik untuk first-party web dalam kondisi domain sesuai; token strategy untuk client lain TBD | CSRF/session/API clients | ADR Required |
| ADR-OPEN-003 | Multi-currency | Dibutuhkan sejak release pertama atau tidak | Model amount, rate, laporan, rounding | TBD |
| ADR-OPEN-004 | Inventory costing | Contoh FIFO/weighted average; tidak dipilih | Valuasi, COGS, reconciliation Finance | ADR Required |
| ADR-OPEN-005 | Payroll scope | Modul penuh atau integrasi awal; yurisdiksi perlu konfirmasi | HRM, Finance, security, regulasi | TBC |
| ADR-OPEN-006 | Collaboration realtime | Kebutuhan realtime sejak awal belum diputuskan | Infrastruktur, delivery, UX | TBD |
| ADR-OPEN-007 | File storage awal | Local atau S3-compatible | Backup, durability, signed URL, deployment | ADR Required |
| ADR-OPEN-008 | Search infrastructure | Search khusus atau PostgreSQL | Query, operasi, scaling | TBD |
| ADR-OPEN-009 | Reporting architecture | Database utama atau read model terpisah | Freshness, isolation, performance | ADR Required |
| ADR-OPEN-010 | Multi-company accounting | Apakah beberapa company dalam satu instalasi | Batas organization, ledger, tax/report | ADR Required |
| ADR-OPEN-011 | Offline POS | Apakah diperlukan | Konsistensi transaksi, payment, stock sync | TBC |
| ADR-OPEN-012 | Workflow branching | Tingkat kompleksitas conditional branching | Engine/state model/version migration | TBD |
| ADR-OPEN-013 | Capacity/availability/recovery | User/concurrency/data, RPO/RTO, availability | Sizing, architecture, backup/monitoring | TBD |
| ADR-OPEN-014 | Final runtime versions | Laravel/PHP/Node/Next/PostgreSQL/Redis compatibility | Build, support, security patches | TBC |

## Format ADR Individual

Setiap keputusan yang diajukan atau diubah harus memiliki file ADR tersendiri berisi ID/title, status (`PROPOSED`, `ACCEPTED`, `REJECTED`, `SUPERSEDED`), context, decision drivers, options, decision, consequences, security/operations impact, owner/approver/date, references, serta update terhadap dokumen terdampak. Status `ACCEPTED` hanya setelah persetujuan manusia tercatat.

## Open Issues

- MTS memuat kemungkinan ambiguitas PostgreSQL: database utama dinyatakan di beberapa bagian, tetapi ADR-006 pada tabel keputusan berstatus Proposed. Minta pemilik arsitektur mengonfirmasi status tanpa mengubah asumsi ini diam-diam.
- Status “Baseline” pada MTS dan status dokumen DRAFT adalah dua dimensi berbeda.

## References / Change History

`../../docs/MTS.md`, §§4, 7–9, 15, 21–22; DOC-007 SAD.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Catatan keputusan MTS dan open-decision log | Documentation Agent |
