# Documentation Standard

| Atribut | Nilai |
|---|---|
| Document Code | GOV-002 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Documentation Manager |
| Related Documents | `documentation/00-governance/document-master-register.md`, `documentation/00-governance/glossary.md`, `documentation/00-governance/documentation-plan.md`, `docs/MTS.md` |

## 1. Tujuan

Menetapkan aturan penulisan, identifikasi, konsistensi, dan pengendalian dokumen BOS. Standar ini mengikuti MTS v1.0 (Draft Baseline) dan instruksi `AGENT.md`; tidak mengubah keputusan teknis maupun bisnis.

## 2. Cakupan

Berlaku untuk DOC-001–DOC-040, dokumen governance, dan seluruh document pack modul pada `documentation/`. Dokumen harus berbahasa Indonesia kecuali istilah teknis, identifier, payload, atau nama resmi yang lebih jelas dalam bahasa sumber.

## 3. Prinsip Sumber Kebenaran

1. `docs/MTS.md` adalah acuan utama arsitektur; `AGENT.md` mengatur cakupan dan kualitas dokumentasi.
2. Dokumen turunan tidak boleh menaikkan status Proposed menjadi keputusan final.
3. Nyatakan jenis pernyataan secara eksplisit: **Fakta MTS**, **Asumsi sementara**, **Rekomendasi**, **TBD**, **TBC**, atau **ADR Required**.
4. Jangan mengarang kebijakan bisnis, akuntansi, payroll, pajak, regulasi, kapasitas, atau operasional.
5. Konflik atau ambiguitas dicatat sebagai open issue beserta opsi/rekomendasi dan pihak yang perlu memutuskan.
6. Gunakan referensi silang ke aturan global untuk mencegah duplikasi yang berbeda.

## 4. Metadata Minimum

Setiap dokumen memuat metadata yang relevan:

| Elemen | Aturan |
|---|---|
| Document Title / Code | Nama resmi dan kode register yang unik |
| Version | Mulai `0.1` untuk draf; naikkan secara terkendali |
| Status | `DRAFT`, `IN REVIEW`, `APPROVED`, atau `DEPRECATED` |
| Created Date / Last Updated | Format `YYYY-MM-DD` |
| Document Owner | Peran/pemilik; gunakan TBD jika belum ditetapkan |
| Related Documents | Kode dan tautan relatif ke dokumen terkait |
| Purpose / Scope | Tujuan serta batas yang eksplisit |
| Definitions & Terminology | Istilah khusus atau rujuk glossary |
| Main Content | Struktur spesifik sesuai jenis dokumen |
| Constraints & Dependencies | Batas MTS dan dependensi |
| Assumptions | Asumsi sementara yang dapat diuji, bila ada |
| Open Issues | ID, uraian, dampak, pemilik keputusan, dan status |
| References | Sumber internal/eksternal yang benar-benar digunakan |
| Change History | Versi, tanggal, perubahan, penulis |

Jangan mengisi bagian yang tidak relevan secara artifisial; tandai “Tidak berlaku” bila membantu pembaca.

## 5. Identifikasi dan Traceability

Gunakan ID unik, stabil, dan bernama domain. Contoh pola dari `AGENT.md`: `BUS-ORG-001`, `SYS-AUTH-001`, `FUN-HRM-001`, `API-CRM-001`, `DB-FIN-001`, `SEC-CORE-001`, `TEST-POS-001`.

Persyaratan minimal menyimpan: ID, uraian, sumber, status/jenis (fakta/asumsi/rekomendasi/TBD), prioritas bila disepakati, pemilik modul, dependensi, verifikasi, serta tautan ke acceptance criteria dan test case. Jangan menetapkan prioritas bisnis tanpa konfirmasi.

Gunakan GOV-005 sebagai traceability requirements-to-design/test setelah dokumen prasyarat tersedia. Uji penerimaan ditautkan ke requirement, use case, komponen/API atau entitas terkait bila relevan, dan test case.

## 6. Aturan Teknis Lintas Dokumen

- Teknologi dan batas arsitektur mengikuti MTS: modular monolith, single-tenant, REST API, Laravel, Next.js/React/TypeScript, PostgreSQL, Redis, Docker Compose, NGINX, Linux. Status teknologi yang masih Proposed harus tetap disebut.
- Basis data: PostgreSQL; dokumentasikan tipe, nullability, PK/FK, unique/index, integritas transaksi, migration, kepemilikan data, dan soft-delete hanya bila dibenarkan. UUID vs ULID belum dipilih.
- API: base path `/api/v1`, JSON, bentuk respons dan kode HTTP mengikuti MTS. Tiap endpoint harus punya ID, method/path, tujuan, authn/authz, parameter/body/response, validasi, status/error, pagination/filter/sort, idempotency, serta audit bila relevan. Jangan menciptakan endpoint tanpa fungsi jelas.
- Security: backend adalah otoritas; pertimbangkan authn, permission, scope data, state bisnis, validasi, audit, dan perlindungan data. Frontend permission-aware bukan kontrol keamanan.
- Integrasi internal menggunakan kontrak yang disetujui; jangan menulis langsung ke tabel domain lain. Event penting mempertimbangkan transactional outbox sesuai MTS.
- Diagram menggunakan Mermaid hanya untuk menjelaskan kondisi yang dijabarkan; label TBD bila kontrak belum diputuskan.
- Contoh JSON harus valid dan contoh data tidak boleh menyiratkan kebijakan nyata yang belum disetujui.

## 7. Dokumen Modul

Setiap pack 12 modul di register harus mencakup 19 topik minimum yang ditetapkan `AGENT.md`. Topik dapat dibagi menjadi file pendukung bila panjangnya perlu, dengan satu overview/index yang memuat status, batas domain, dependensi, dan tautan. Aturan global dirujuk; entitas/API/permission/workflow khusus modul harus konsisten dengan dokumen global terkait. Belum boleh menyatakan skema/API final hanya dari nama fitur dalam MTS.

## 8. Status dan Persetujuan

- `DRAFT`: konten disusun, belum disahkan.
- `IN REVIEW`: peninjau dan lingkup review dicatat.
- `APPROVED`: hanya setelah persetujuan manusia yang tercatat (nama/peran, tanggal, versi/lingkup).
- `DEPRECATED`: tidak berlaku; tautkan pengganti.

Agen penyusun tidak dapat menyatakan dokumen `APPROVED` tanpa proses persetujuan manusia. Status register dan metadata dokumen harus selaras.

## 9. Review Mutu Sebelum Publikasi

Periksa kesesuaian MTS, scope, ID unik, konsistensi istilah/model/API/permission/workflow, asumsi dan TBD, dependensi, diagram, traceability, implementability, serta referensi. Catat isu yang belum selesai; jangan menyamarkannya sebagai keputusan.

## 10. Batasan dan Dependensi

Bergantung pada keputusan pemilik bisnis, arsitektur, keamanan, operasi, dan data yang belum tersedia. Batasan prioritas: tidak mengubah single-tenant menjadi multi-tenant, tidak mengadopsi microservices sebagai arsitektur awal, dan tidak mengarang kebijakan bisnis.

## 11. Open Issues

| ID | Isu | Status |
|---|---|---|
| GOV-OPEN-001 | Pemilik dokumen dan approver per domain belum ditetapkan | TBD |
| GOV-OPEN-002 | Template prioritas requirement dan siklus review formal perlu dikonfirmasi | TBC |
| GOV-OPEN-003 | Penyelesaian keputusan terbuka MTS harus dicatat melalui ADR/keputusan bisnis terkait | ADR Required |

## 12. References

- `AGENT.md`, bagian 8–18.
- `docs/MTS.md`, v1.0 Draft Baseline.
- `documentation/00-governance/document-master-register.md`.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Standar dokumentasi awal untuk cakupan BOS | Documentation Agent |
