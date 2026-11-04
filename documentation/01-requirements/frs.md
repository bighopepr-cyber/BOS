# Functional Requirements Specification (FRS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-003 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Product Owner |
| Related Documents | `brs.md`, `srs.md`, `use-case-specification.md`, `business-process-specification.md`, `../00-governance/glossary.md` |

## 1. Tujuan dan Konvensi

Mendefinisikan kapabilitas fungsional yang diturunkan dari MTS. “Harus” berarti requirement baseline; “dapat” berarti kapabilitas yang diakomodasi tetapi aktivasi/rinciannya belum final. ID requirement stabil dan ditautkan ke DOC-001/DOC-002. Detail transaksi, formula, field, role, dan endpoint yang belum ditetapkan tetap TBD.

## 2. Requirements per Modul

### 2.1 Core System

| ID | Requirement | Trace |
|---|---|---|
| FUN-ORG-001 | Kelola profil organisasi serta struktur business unit, branch, division, department, team, position dan relasi parent-child; tingkat organisasi bersifat opsional. | BUS-ORG-001; SYS-ORG-001 |
| FUN-ID-001 | Kelola user, profile, autentikasi, password reset, status akun, session, dan riwayat login. User terpisah dari Employee. | BUS-CORE-001; SYS-ID-001 |
| FUN-AUTH-001 | Kelola Role dan Permission dengan granularitas `module.resource.action`; role bawaan dapat dikonfigurasi. | BUS-ACCESS-001; SYS-AUTH-001 |
| FUN-AUTH-002 | Terapkan data scope organisasi, unit, cabang, departemen, own record, atau project assignment sesuai grant yang berlaku. | BUS-ACCESS-001; SYS-AUTH-002 |
| FUN-WF-001 | Konfigurasikan workflow definition/version, step, approver, conditional routing, delegation, rejection/revision, escalation, dan history. | BUS-WORK-001; SYS-WF-001 |
| FUN-NOTIF-001 | Sediakan in-app notification; dukung email/webhook channel, template, preference, read state, delivery status, history, retry. | BUS-CORE-001; SYS-NOTIF-001 |
| FUN-AUD-001 | Catat aktivitas penting beserta actor/action/entity/time/request ID/context dan before/after state bila sesuai; jangan merekam credential/secret. | BUS-AUD-001; SYS-AUD-001 |
| FUN-DOC-001 | Simpan file/version/category/relationship dan proteksi akses file sensitif. | BUS-DATA-001; SYS-DOC-001 |
| FUN-NUM-001 | Konfigurasikan prefix/suffix/periode/sequence/reset policy dan scope sequence bila dipilih; cegah nomor ganda concurrent. | BUS-DATA-001; SYS-NUM-001 |
| FUN-CF-001 | Definisikan custom field bertipe text, number, date, boolean, select, multi-select, atau URL pada entitas yang mendukungnya. | BUS-DATA-001; SYS-CF-001 |
| FUN-IMP-001 | Impor/ekspor CSV/XLSX dan ekspor PDF sesuai fungsi; import menyediakan mapping, validation, preview, duplicate detection, dan error report. | BUS-CORE-001; SYS-IMP-001 |
| FUN-CONF-001 | Kelola konfigurasi operasional organisasi, workflow, numbering, locale/timezone/currency, notification dan parameter modul dengan kontrol akses. Nilai awal TBD. | BUS-OPS-001 |

### 2.2 Business Modules

| ID | Requirement | Trace |
|---|---|---|
| FUN-HRM-001 | Kelola employee profile/number, employment, posisi/unit, manager, status, contract history, dokumen, emergency contact dan lifecycle. | BUS-HRM-001 |
| FUN-HRM-002 | Sediakan cakupan recruitment, attendance/shift/overtime, leave/balance/approval, payroll period/calculation/payslip, performance/KPI, dan training record. Formula payroll/regulasi TBD. | BUS-HRM-001 |
| FUN-FIN-001 | Kelola chart of accounts, fiscal/accounting period, journal entry/line, posting, reversal/adjustment, GL, trial balance, closing, serta financial reports. | BUS-FIN-001 |
| FUN-FIN-002 | Kelola AR invoice/payment/credit note/aging; AP bill/payment/vendor credit/aging; cash/bank/reconciliation; expense claims; budget/variance/approval. | BUS-FIN-001 |
| FUN-CRM-001 | Kelola lead/source/assignment/scoring/qualification/conversion, customer/contact profile dan history hubungan. Formula scoring dan field wajib TBD. | BUS-CORE-001 |
| FUN-CRM-002 | Kelola sales pipeline/stage/opportunity/probability/forecast dan aktivitas call/meeting/email/note/follow-up. | BUS-CORE-001 |
| FUN-SALES-001 | Kelola quotation/item/discount/tax/validity/approval/PDF dan sales order/item/status dengan handoff invoice. Tax dan approval policy TBD. | BUS-INTEG-001 |
| FUN-PROJ-001 | Kelola project/client/code/manager/member/budget/timeline/status, task/subtask/assignee/dependency/checklist/comment/attachment, milestone/planning/resource allocation. | BUS-CORE-001 |
| FUN-PROJ-002 | Kelola timesheet/worklog/billable hours/approval, project cost, expense allocation, revenue/profitability sesuai kebijakan TBD. | BUS-CORE-001 |
| FUN-COL-001 | Sediakan announcement, discussion, comments, direct/group message, calendar, meeting, shared documents, activity feed, mention, notification dengan akses sesuai konteks organisasi/proyek. | BUS-CORE-001 |
| FUN-INV-001 | Kelola product/category/SKU/UOM, warehouse/location, balance, movement, transfer dispatch/receipt, adjustment, stock opname, reorder level, batch/lot/serial bila relevan, valuation. | BUS-INV-001 |
| FUN-INV-002 | Sajikan saldo sebagai read model/hasil konsisten dari stock movement; costing policy tidak ditentukan oleh requirement ini. | BUS-INV-001; SYS-INV-001 |
| FUN-POS-001 | Kelola terminal/cashier shift/catalog/cart/discount/tax/payment/sales transaction/receipt/return/refund/cash closing/daily report; lindungi checkout dari duplikasi. | BUS-POS-001; SYS-POS-001 |
| FUN-LMS-001 | Kelola course/category/learning path/lesson/material/enrollment/assignment/quiz/assessment/certification/progress/completion dan relasi employee. | BUS-CORE-001 |
| FUN-ANL-001 | Sediakan dashboard/KPI/filter/date range/drill-down dan report per domain; ekspor CSV/XLSX/PDF serta scheduled reports sesuai izin dan konfigurasi. | BUS-REPORT-001; SYS-REP-001 |

### 2.3 System Administration

| ID | Requirement | Trace |
|---|---|---|
| FUN-ADM-001 | Mendukung install/setup: pemeriksaan prasyarat, konfigurasi environment, DB migration, initial admin/organization, storage/email, health check, installation lock. | BUS-OPS-001 |
| FUN-ADM-002 | Administer user, module/configuration, queue, scheduler, storage, application version, DB, worker, backup status dan recent errors berdasarkan permission. | BUS-OPS-001; SYS-OPS-001 |
| FUN-ADM-003 | Dukung update melalui validasi versi, backup, maintenance bila diperlukan, migration, cache clear, worker restart, health check, dan pencatatan hasil. | BUS-OPS-001 |
| FUN-ADM-004 | Dukung backup dan pemulihan database, uploaded files, konfigurasi, dan material kunci melalui prosedur penyimpanan aman. | BUS-OPS-001 |

## 3. Aturan Umum Interaksi

1. Semua aksi terlindungi diperiksa di backend; frontend hanya menyesuaikan tampilan.
2. Mutasi multi-entitas yang harus atomik menggunakan transaksi database.
3. Proses asynchronous menggunakan queue; detail retry/timeout/error ada di DOC-020.
4. Integrasi antar modul melalui kontrak; kepemilikan data harus terjaga.
5. Setiap endpoint, field, state transition, permission, dan validation spesifik akan dirinci di dokumen turunan/API modul setelah desain disetujui.

## 4. Constraints, Assumptions, dan Open Issues

- MTS §6 memberikan daftar capability, bukan seluruh definisi field dan alur final. FRS tidak menganggap setiap subfitur wajib tersedia pada release pertama; prioritas release TBD.
- Kebijakan pajak, mata uang, payroll, cost accounting, discount, retensi, akses data, channel external, serta offline POS TBD.
- Riwayat dan ledger sensitif harus dipertahankan sesuai aturan domain; retensi/penghapusan spesifik belum ditentukan.

## 5. References

- `brs.md` DOC-001; `srs.md` DOC-002.
- `../../docs/MTS.md`, §§5–16 dan §20.
- `../00-governance/documentation-standard.md`.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | FRS awal semua core/business/admin capability menurut MTS | Documentation Agent |
