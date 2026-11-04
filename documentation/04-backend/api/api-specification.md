# API Specification (API-SPEC)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-019 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — API Owner |
| Related Documents | `../backend-technical-specification.md`, `../authentication-authorization.md`, `../../03-data/data-dictionary.md`, `../../02-architecture/integration-architecture.md`, `../../../docs/MTS.md` |

## 1. Global Contract

MTS baseline: HTTP JSON UTF-8, base path `/api/v1`. Authentication dan authorization berlaku di backend. Collection memakai pagination (`page`, `per_page`, `sort`, `direction`, `search` dan filter yang diizinkan). Breaking change menggunakan versi baru atau migrasi terdokumentasi. Idempotency untuk operasi rawan duplikasi.

Success:
```json
{"success":true,"data":{},"meta":{},"message":"Operation successful"}
```
Error:
```json
{"success":false,"error":{"code":"VALIDATION_ERROR","message":"Request validation failed","details":{}}}
```

HTTP status mengikuti MTS: 200, 201, 202, 204, 400, 401, 403, 404, 409, 422, 429, 500. Jangan mengembalikan credential/secret atau internal exception.

## 2. Endpoint Contracts (Konseptual)

Path/entity payload di bawah adalah kandidat berdasar contoh MTS; field schema dan URL final wajib direview bersama modul sebelum implementasi.

### API-ID-001 — Membuat sesi login

- **Method/URL:** `POST /api/v1/auth/login` (candidate; route exact TBD)
- **Tujuan:** autentikasi first-party user.
- **AuthN:** tidak perlu session aktif; throttling wajib.
- **AuthZ:** endpoint publik terbatas rate; akun suspended ditolak.
- **Request body:** credential fields sesuai identifier/password policy TBD; contoh tidak menetapkan format identifier.
- **Response:** 200 standard envelope, data berisi hasil minimal dan session/cookie sesuai ADR-OPEN-002; token strategy tidak diputuskan.
- **Validasi:** required credential, format input, throttling.
- **Errors:** 400/422 request malformed, 401 generic credential failure, 429 limit.
- **Pagination/filter/sort:** tidak relevan. **Idempotency:** tidak relevan. **Audit:** login success/failure tanpa mencatat password/token.

### API-ORG-001 — Membuat unit/cabang organisasi

- **Method/URL:** `POST /api/v1/organization-units` (candidate; resource shape TBD)
- **Tujuan:** menambahkan node organisasi dengan parent/type sesuai konfigurasi.
- **AuthN:** wajib.
- **AuthZ:** permission organization create/update + scope administrative; catalog final TBD.
- **Request:** candidate JSON `{"name":"...","type":"...","parent_id":"..."}`. `id` type sengaja tidak dinyatakan.
- **Response:** 201 standard envelope dengan resource hasil.
- **Validasi:** nama/type/parent required sesuai level yang digunakan; mencegah parent invalid/cycle; exact fields TBD.
- **Errors:** 401/403/404/409/422/500.
- **Pagination/filter/sort:** create tidak berlaku; collection listing pada endpoint terpisah memakai global pagination.
- **Idempotency:** tidak wajib kecuali import retry policy memerlukan. **Audit:** perubahan struktur dicatat.

### API-WF-001 — Memutuskan approval

- **Method/URL:** `POST /api/v1/workflow-instances/{id}/decision` (candidate)
- **Tujuan:** approver mengirim keputusan workflow.
- **AuthN:** wajib.
- **AuthZ:** assigned/authorized approver, record scope, workflow state.
- **Request:** `{"decision":"approve|reject|revision","comment":"..."}`; enumerasi final TBD.
- **Response:** 200 envelope dengan workflow/business status minimal.
- **Validasi:** decision tersedia pada state/version aktif; comment policy TBD; concurrency check wajib.
- **Errors:** 401/403/404/409 stale/invalid state/422/500.
- **Idempotency:** concurrency/version protection; retry behavior ditetapkan. **Audit:** approval history wajib.

### API-FIN-001 — Posting journal

- **Method/URL:** `POST /api/v1/journal-entries/{id}/post` (candidate)
- **Tujuan:** validasi dan posting journal draft.
- **AuthN:** wajib.
- **AuthZ:** `finance.journal.post` candidate permission + data scope + period state.
- **Request:** `POST` transition; idempotency key disarankan/diwajibkan sesuai desain payment-like operation. Tidak ada body bisnis minimum yang diputuskan.
- **Response:** 200/202 sesuai approval/async contract; status final TBD.
- **Validasi:** debit=credit, period open, referensi valid, concurrency/version; monetary precision TBD.
- **Errors:** 401/403/404/409/422/500.
- **Pagination:** tidak relevan. **Audit:** source reference dan actor dicatat; posted tidak dapat diedit langsung.

## 3. Endpoint Inventory Rules

DOC-019 per modul harus melengkapi setiap endpoint dengan Endpoint ID, method/path, purpose, authn/authz, request parameters/body, response, validation, status/error, pagination/filter/sort, idempotency dan audit. Tidak ada CRUD endpoint yang otomatis dibuat dari nama tabel. Route/resource naming, OpenAPI version, date/time format, currency format, maximum page size, filter allowlist, ETag/concurrency token, API deprecation policy masih TBD.

## 4. Open Issues

Authentication scheme/cookie domain, UUID/ULID in paths, final resource contracts, field-level scope, pagination caps/cursor policy, standard error code catalog, idempotency storage/expiry, API documentation generation tool, rate limit quotas and version sunset.

## References / Change History

MTS §8–9; DOC-018, DOC-021; DOC-010/011/015.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Standar API dan contoh kontrak kandidat berbasis use case MTS | Documentation Agent |
