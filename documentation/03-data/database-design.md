# Database Design Specification (DDS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-013 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Database Architect |
| Related Documents | `../02-architecture/module-architecture/module-architecture-specification.md`, `../02-architecture/module-dependency-matrix.md`, `entity-relationship.md`, `data-dictionary.md`, `../../docs/MTS.md` |

## 1. Tujuan dan Baseline

Menetapkan desain database konseptual. MTS menyatakan PostgreSQL sebagai database utama namun status ADR-006 tercatat Proposed; klarifikasi status wajib sebelum implementasi. Skema tabel berikut belum disahkan dan bukan migration contract.

## 2. Prinsip

- Relational model, FK untuk relasi penting, unique constraints untuk natural/business uniqueness yang dikonfirmasi, index berdasarkan workload terukur.
- Migration menjadi sumber perubahan skema; review schema dan data backfill sebelum release.
- Transaksi untuk perubahan atomik; ownership data per modul mengikuti DOC-010.
- JSON tidak menggantikan relasi inti. JSONB hanya bila use case/configuration mengizinkan.
- Soft delete per tabel berdasarkan kebutuhan; bukan default global. Financial/ledger history tidak dihapus lewat proses normal.
- PK strategy belum dipilih (UUID/ULID/strategi konsisten). Hindari asumsi tipe ID pada API/DDL final.

## 3. Entity Groups Awal dari MTS

- Identity/Access: users, user_profiles, user_sessions, password_reset_tokens, roles, permissions, role_permissions, user_roles.
- Organization: organizations, business_units, branches, divisions, departments, teams, positions, organization_hierarchies.
- Workflow/Notification/Audit/Documents: workflow_definitions/versions/steps/instances/approvals/histories; notifications/templates/preferences/deliveries; audit_logs/activity_logs; files/file_versions/document_categories/document_relations.
- Finance: chart_of_accounts, accounting_periods, fiscal_years, journal_entries, journal_lines, bank_accounts, payments, invoices, bills.
- Inventory: products, product_categories, units_of_measure, warehouses, warehouse_locations, stock_movements, stock_movement_lines, stock_balances, stock_transfers, stock_adjustments, inventory_valuations.
- Entitas domain lain dirinci saat module packs dan keputusan ownership tersedia.

## 4. Constraint/Index Design Rules

Untuk setiap entitas fisik DOC-014/015 wajib menetapkan: PK type, FK target and referential action, nullable, uniqueness scope, check constraints, indexes, audit fields, soft-delete/retention, owner, write path, migration/backfill. Kandidat unique key (mis. employee number per organization, SKU, document number, role/permission name) perlu konfirmasi scope dan aturan reuse sebelum constraint final.

Indeks dirancang dari query/API/report patterns; ukur selectivity dan write impact. Jangan menambah index spekulatif untuk semua field. FK yang sering dipakai join/filter dan unique lookup perlu dievaluasi. Partitioning/read replica tidak diputuskan pada baseline.

## 5. Domain Invariants dan Transaksi

- Journal post: debit=credit, period valid/unlocked, transaksi atomic; money numeric presisi, precision/scale TBD.
- Posted journal tidak diedit; reversal/adjustment menyimpan referensi sumber.
- Stock movement menjadi sumber riwayat; saldo sebagai read model/derived balance harus diperbarui konsisten.
- Transfer memiliki dispatch dan receipt; failure/partial receipt rules TBD.
- Number sequence harus concurrent-safe dan tidak duplikat.
- Core audit/history append-only secara kebijakan; implementasi storage/retention TBD.

## 6. Migrations

Migration harus bernomor/terurut, idempotent pada konteks runner yang didukung, ter-review, aman untuk data volume, dan dipisah bila perlu antara schema/backfill. Production change meliputi backup, compatibility check, lock/downtime assessment, post-migration validation, serta recovery plan. Migrations tidak menjanjikan rollback aman untuk perubahan destruktif. Detail deployment ada DOC-028/037.

## 7. Open Issues

UUID vs ULID; ADR-006 status PostgreSQL; multi-company; currency precision; soft delete/retention; exact audit ledger strategy; organization hierarchy constraints; table schema, column types, indexes, partitioning and volume; migration tool/runtime versions; RPO/RTO.

## References / Change History

MTS §§4.3, 7, 15.4, 16.4; DOC-010/012, DOC-014/015/016/017.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Baseline database dan aturan desain, belum final DDL | Documentation Agent |
