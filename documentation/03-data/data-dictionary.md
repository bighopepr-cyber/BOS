# Data Dictionary (DD)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-015 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Data Owners |
| Related Documents | `database-design.md`, `entity-relationship.md`, `data-governance.md`, `../../docs/MTS.md` |

## 1. Tujuan dan Konvensi

Kamus awal istilah entity dan atribut konseptual dari MTS. Nama tabel mengikuti lowercase plural snake_case, kolom snake_case, FK `<entity>_id`. Kolom standar dapat mencakup id, created_at, updated_at, created_by, updated_by; delete fields hanya untuk tabel yang memerlukannya. Tipe fisik/nullability/PK belum final.

## 2. Kamus Entitas

| Entitas | Modul owner | Makna/atribut sumber MTS | Catatan |
|---|---|---|---|
| organizations | Organization | Profil organisasi | Satu instalasi per organisasi; hubungan dengan company akuntansi TBD |
| business_units | Organization | Unit bisnis internal | Hierarchy/required level dikonfigurasi |
| branches | Organization | Cabang | Scope/sequence policy TBD |
| divisions, departments, teams, positions | Organization | Struktur organisasi internal | Level opsional; model hierarchy final TBD |
| users | Identity | Akun autentikasi | Tidak sama dengan employee; unique identifier TBD |
| user_profiles | Identity | Informasi profil user | Data pribadi/classification TBD |
| user_sessions | Identity | Session aktif/riwayat sesuai implementasi | Session/token strategy dan retention TBD |
| roles | Access Control | Kumpulan permission | Bawaan dapat dikonfigurasi; scope TBD |
| permissions | Access Control | Granularitas `module.resource.action` | Katalog final dibuat per modul |
| user_roles | Access Control | Assign user ke role | Scope dan validity period TBD |
| role_permissions | Access Control | Permission yang dimiliki role | Unique pair diharapkan, perlu schema review |
| workflow_definitions / workflow_versions | Workflow | Definisi alur dan versinya | Publish/effective version policy TBD |
| workflow_steps | Workflow | Langkah/approval routing | Conditional rule schema TBD |
| workflow_instances | Workflow | Instance workflow proses bisnis | Target entity reference design TBD |
| workflow_approvals / histories | Workflow | Keputusan dan sejarah transisi | Retention/integrity policy TBD |
| notifications / notification_deliveries | Notification | Notifikasi dan status delivery | Channel/provider/status enum TBD |
| audit_logs / activity_logs | Audit | Jejak aktivitas | Minimum fields actor, action, entity type/id, timestamp, request ID, context; secrets prohibited |
| files / file_versions | Documents | Metadata file dan versi | Binary di storage luar public root; MIME/size policy TBD |
| document_categories / document_relations | Documents | Kategori dan hubungan file ke entitas | Polymorphic relation/security design TBD |
| chart_of_accounts | Finance | Struktur akun | Accounting policy/code hierarchy TBD |
| accounting_periods / fiscal_years | Finance | Periode akuntansi/tahun fiskal | Calendar/closing/reopen policy TBD |
| journal_entries | Finance | Header jurnal, state, source ref | Posted tidak diedit langsung; currency fields TBD |
| journal_lines | Finance | Baris debit/kredit, account, description, dimensions/reference bila perlu | Debit/credit numeric presisi, precision TBD |
| invoices / bills / payments | Finance | AR/AP invoices/bills dan payment | Tax/currency/integration fields TBD |
| products / product_categories | Inventory | Barang/SKU/category | Product master ownership dengan Sales/POS TBD |
| units_of_measure | Inventory | Unit pengukuran | Conversion rules TBD |
| warehouses / warehouse_locations | Inventory | Gudang dan lokasi | Hierarchy/unique code TBD |
| stock_movements / stock_movement_lines | Inventory | Riwayat perubahan stok dan detail item | Sumber kebenaran gerak; immutable correction policy TBD |
| stock_transfers / stock_adjustments | Inventory | Transfer dan adjustment | Dispatch/receipt; approval/audit wajib sesuai proses |
| stock_balances | Inventory | Saldo read model/derived | Konsistensi transaksi/rebuild strategy TBD |
| inventory_valuations | Inventory/Finance | Nilai persediaan | Costing method TBD; tidak boleh diasumsikan |

## 3. Atribut Finansial

Jumlah uang harus menggunakan tipe numerik presisi, bukan floating point. Precision/scale, currency code, FX rate, rounding, nullable, sign convention, dan check constraints perlu ditetapkan pada level kebijakan/ADR. Nilai debit/kredit dan currency tidak dapat difinalisasi hanya dari MTS.

## 4. Atribut Audit

Untuk tabel operasional, created/updated timestamps dan actor references dapat digunakan sesuai kebutuhan. `deleted_at`/`deleted_by` hanya jika soft delete dibenarkan. Audit log sebaiknya meminimalkan data sensitif dan tidak menyimpan password, token, secret. Bentuk before/after snapshot, redaction, partition, retention dan immutable storage TBD.

## 5. Open Issues

Physical PostgreSQL types, PK/FK types, nullability, unique/index constraints, enum/reference tables, data classification, retention/deletion, personal/financial sensitive fields, custom field schema, polymorphic references, audit storage/partitioning, and integration identifiers require detailed schema review.

## References / Change History

MTS §§5–7; DOC-013, DOC-014, DOC-016.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Kamus entity awal berdasarkan daftar MTS | Documentation Agent |
