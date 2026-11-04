MASTER TECHNICAL SPECIFICATION (MTS)

Business Operating System — Self-Hosted Edition

Dokumen: Master Technical Specification
Versi: 1.0
Status: Draft Baseline
Arsitektur: Self-Hosted · Single-Tenant · Modular Monolith
Platform: Web Application
Bahasa Dokumen: Indonesia

---

1. PENDAHULUAN

1.1 Tujuan Dokumen

Master Technical Specification (MTS) merupakan dokumen induk yang mendefinisikan spesifikasi teknis dan arsitektur sistem Business Operating System (BOS).

Dokumen ini menjadi acuan untuk:

1. Menentukan arsitektur perangkat lunak.
2. Menentukan teknologi dan standar implementasi.
3. Mendefinisikan batas dan tanggung jawab setiap modul.
4. Menetapkan standar desain database.
5. Menentukan pola komunikasi antar modul.
6. Menentukan standar API.
7. Menetapkan mekanisme autentikasi dan otorisasi.
8. Menentukan kebutuhan infrastruktur.
9. Menentukan standar keamanan dan performa.
10. Menetapkan standar pengujian dan deployment.

MTS menjadi dasar bagi dokumen turunan, seperti:

- Software Requirements Specification (SRS).
- Software Architecture Document (SAD).
- Database Design Specification (DDS).
- API Specification.
- UI/UX Specification.
- Module Technical Specification.
- Test Plan and Test Case.
- Deployment and Operations Manual.

1.2 Gambaran Produk

Business Operating System adalah sistem terintegrasi untuk mengelola proses operasional organisasi melalui satu aplikasi.

Sistem mengintegrasikan proses:

- Manajemen organisasi.
- Human Resource Management.
- Finance and Accounting.
- Customer Relationship Management.
- Sales Management.
- Project Management.
- Collaboration.
- Inventory Management.
- Point of Sale.
- Learning Management.
- Analytics and Reporting.
- System Administration.

Sistem dirancang agar berbagai fungsi bisnis dapat menggunakan data dan layanan inti yang sama, tanpa membuat duplikasi sumber data yang tidak diperlukan.

1.3 Ruang Lingkup Arsitektur

Sistem memiliki karakteristik:

Aspek| Spesifikasi
Deployment| Self-hosted
Tenant model| Single-tenant
Organization| Mendukung banyak unit organisasi
Architecture| Modular monolith
Backend| Laravel
Frontend| Next.js, React, TypeScript
Database| PostgreSQL
Cache| Redis
Queue| Redis-based queue
API| REST API
Communication| Synchronous API dan asynchronous events
Container| Docker Compose
Reverse proxy| NGINX
Operating system| Linux
File storage| Local storage atau S3-compatible storage

1.4 Di Luar Ruang Lingkup

Versi ini tidak mencakup:

- SaaS multi-tenant infrastructure.
- Tenant provisioning.
- Tenant billing.
- Subscription management.
- SaaS marketplace.
- SaaS usage metering.
- Platform-level tenant administration.
- Microservices sebagai arsitektur awal.

---

2. ARCHITECTURAL PRINCIPLES

2.1 Single-Tenant Architecture

Setiap instalasi aplikasi digunakan oleh satu organisasi atau satu lingkungan operasional.

Seluruh data bisnis disimpan dalam satu database instalasi, dengan pemisahan data berdasarkan konteks organisasi internal seperti cabang, departemen, dan unit bisnis.

Tidak diperlukan:

- Tenant ID pada setiap tabel hanya untuk isolasi antar pelanggan.
- Tenant database resolver.
- Tenant provisioning service.
- Tenant routing middleware.

Namun, struktur organisasi tetap harus diterapkan untuk mendukung pembatasan akses data internal.

2.2 Modular Monolith

Sistem dibangun sebagai satu aplikasi backend dengan modul domain yang terpisah secara logis.

Setiap modul memiliki:

- Domain model.
- Business rules.
- Application services.
- Data access layer.
- API endpoints.
- Events.
- Permissions.
- Tests.

Modul tidak boleh mengakses tabel internal modul lain secara sembarangan.

Interaksi lintas modul harus menggunakan kontrak yang disepakati, seperti application service, domain event, atau public interface modul.

2.3 API-First

Seluruh fungsi bisnis utama harus dapat diakses melalui API yang terstruktur.

API digunakan oleh:

- Frontend utama.
- Aplikasi mobile di masa mendatang.
- Integrasi eksternal.
- Sistem internal lain yang memiliki otorisasi.

2.4 Configuration-Driven

Pengaturan yang bersifat operasional harus dapat dikelola melalui konfigurasi sistem apabila memungkinkan.

Contohnya:

- Struktur organisasi.
- Jam kerja.
- Approval workflow.
- Status transaksi.
- Nomor dokumen.
- Format tanggal.
- Mata uang.
- Pengaturan notifikasi.
- Parameter modul.

Konfigurasi tidak boleh digunakan untuk menggantikan business logic yang membutuhkan validasi dan kontrol integritas.

2.5 Security by Design

Keamanan harus menjadi bagian dari setiap lapisan aplikasi, bukan fitur tambahan setelah aplikasi selesai.

Prinsip:

- Least privilege.
- Deny by default.
- Server-side authorization.
- Validasi input.
- Auditability.
- Secure session management.
- Encryption in transit.
- Protection of sensitive data.

---

3. SYSTEM ARCHITECTURE

3.1 High-Level Architecture

Arsitektur sistem dibagi menjadi enam lapisan.

A. Presentation Layer

Bertanggung jawab terhadap tampilan dan interaksi pengguna.

Komponen:

- Web application.
- Dashboard.
- Navigation.
- Forms.
- Tables.
- Charts.
- Notifications.
- User settings.
- Administrative interface.

Teknologi:

- Next.js.
- React.
- TypeScript.
- Tailwind CSS.
- Component library yang konsisten.
- Data-fetching client.

B. API Layer

Bertanggung jawab terhadap komunikasi frontend dan backend.

Komponen:

- API routing.
- Request validation.
- Authentication middleware.
- Authorization middleware.
- Rate limiting.
- Response formatting.
- API versioning.
- Exception handling.

C. Application Layer

Bertanggung jawab terhadap pelaksanaan use case bisnis.

Contoh:

- CreateEmployee.
- ApproveLeaveRequest.
- CreateSalesOrder.
- PostJournalEntry.
- AssignProjectTask.
- TransferInventory.

Application service mengatur alur bisnis dan koordinasi domain.

D. Domain Layer

Berisi aturan bisnis inti.

Komponen:

- Entities.
- Value objects.
- Domain services.
- Domain events.
- Business policies.
- Invariants.

Domain layer tidak boleh bergantung langsung pada HTTP request, komponen UI, atau framework frontend.

E. Infrastructure Layer

Menangani kebutuhan teknis.

Komponen:

- Database repository.
- Cache.
- Queue.
- File storage.
- Email provider.
- External API client.
- Scheduler.
- Logging.
- Search infrastructure bila dibutuhkan.

F. Deployment Layer

Menangani proses operasional aplikasi.

Komponen:

- Linux server.
- Docker.
- Docker Compose.
- NGINX.
- PHP runtime.
- Node runtime.
- PostgreSQL.
- Redis.
- Backup service.
- Monitoring.

3.2 Arsitektur Komunikasi

Alur permintaan normal:

1. User melakukan tindakan di frontend.
2. Frontend mengirim HTTP request ke backend.
3. Middleware memeriksa session atau token.
4. Authorization memeriksa hak akses.
5. Request divalidasi.
6. Application service menjalankan use case.
7. Domain menjalankan aturan bisnis.
8. Repository membaca atau mengubah data.
9. Database menyimpan perubahan.
10. Response dikembalikan kepada frontend.

Alur proses asynchronous:

1. Application service menjalankan transaksi.
2. Event dicatat atau dipublikasikan setelah transaksi berhasil.
3. Queue worker menerima pekerjaan.
4. Handler menjalankan proses lanjutan.
5. Hasil dicatat pada log atau tabel terkait.
6. Notifikasi dikirim bila diperlukan.

Untuk event penting, gunakan pola transactional outbox agar perubahan database dan publikasi event tidak mudah kehilangan sinkronisasi.

3.3 Batas Antar Modul

Setiap modul harus memiliki batas tanggung jawab yang jelas.

Contoh:

Modul| Pemilik data utama
Organization| Struktur organisasi
Identity| User dan credential
Access Control| Role dan permission
HRM| Employee dan employment
Finance| Account, journal, ledger
CRM| Lead dan customer relationship
Sales| Quotation dan sales order
Project| Project dan task
Inventory| Product stock dan warehouse
POS| Retail transaction
LMS| Course dan learning record

Modul lain boleh membaca data melalui kontrak yang tersedia, tetapi tidak boleh mengubah data milik modul lain secara langsung.

---

4. TECHNOLOGY STACK

4.1 Backend

Framework: Laravel
Language: PHP
API style: RESTful JSON API

Tanggung jawab:

- Business logic.
- API.
- Authentication.
- Authorization.
- Validation.
- Database transaction.
- Queue.
- Scheduler.
- Event handling.
- File management.
- Integration services.

Standar:

- Gunakan Laravel Form Request untuk validasi request.
- Gunakan Policy atau authorization service untuk otorisasi.
- Gunakan service/application layer untuk use case kompleks.
- Gunakan database transaction untuk proses multi-tabel yang harus atomik.
- Gunakan event dan queue untuk proses asynchronous.
- Gunakan dependency injection.
- Hindari business logic berat di controller.

Versi Laravel dan PHP harus dikunci berdasarkan versi stabil yang kompatibel ketika implementasi dimulai.

4.2 Frontend

Framework: Next.js
UI: React
Language: TypeScript

Tanggung jawab:

- Rendering antarmuka.
- Navigation.
- Form management.
- Client-side state.
- Data fetching.
- Dashboard.
- Interactive components.
- Permission-aware interface.

Standar:

- Strict TypeScript.
- Reusable components.
- Feature-based organization.
- Centralized API client.
- Consistent error handling.
- Form validation.
- Responsive layout.
- Accessible interaction.

Frontend tidak boleh menjadi sumber otoritas permission. Pemeriksaan hak akses wajib dilakukan di backend.

4.3 Database

Database utama: PostgreSQL.

Alasan:

- Relational integrity.
- Transaction support.
- Advanced indexing.
- JSONB support.
- Reporting query capability.
- Strong data consistency.

Standar:

- Primary key menggunakan UUID atau strategi ID yang ditetapkan secara konsisten.
- Foreign key wajib digunakan untuk relasi penting.
- Unique constraints diterapkan pada data unik.
- Index disesuaikan dengan pola query.
- Migration menjadi sumber perubahan skema.
- Soft delete hanya digunakan jika kebutuhan bisnis membenarkannya.
- Data keuangan tidak dihapus secara fisik melalui proses normal.

4.4 Cache dan Queue

Redis digunakan untuk:

- Cache.
- Queue.
- Distributed locks bila diperlukan.
- Temporary state.
- Rate limiting.

Queue digunakan untuk:

- Email.
- Notification.
- Report generation.
- File processing.
- Import dan export.
- Event handlers.
- Scheduled background tasks.

Job harus memiliki mekanisme retry, timeout, logging, dan penanganan kegagalan.

4.5 File Storage

Sistem mendukung:

- Local filesystem storage.
- S3-compatible object storage.

File disimpan di luar public web root.

Akses file sensitif harus melalui authorization check atau signed URL dengan masa berlaku terbatas.

4.6 Infrastructure

Komponen:

- Linux.
- Docker.
- Docker Compose.
- NGINX.
- PostgreSQL.
- Redis.
- PHP-FPM.
- Next.js runtime.
- Queue worker.
- Scheduler.

Konfigurasi production harus memisahkan secret, konfigurasi aplikasi, dan data persisten.

---

5. CORE SYSTEM ARCHITECTURE

Core System adalah fondasi yang digunakan seluruh modul.

5.1 Organization Management

Fitur:

- Organization profile.
- Business identity.
- Branch.
- Business unit.
- Division.
- Department.
- Team.
- Position.
- Reporting hierarchy.
- Organizational structure.

Struktur konseptual:

Organization
→ Business Unit
→ Branch
→ Division
→ Department
→ Team
→ Position

Struktur tersebut harus fleksibel. Organisasi tidak wajib menggunakan seluruh level.

Data organisasi dapat menggunakan parent-child hierarchy agar organisasi dapat mengatur struktur sesuai kebutuhan.

5.2 Identity Management

Fitur:

- User account.
- Login.
- Logout.
- Password management.
- Password reset.
- Session management.
- Account activation.
- Account suspension.
- User profile.
- Login history.
- Multi-factor authentication sebagai kemampuan yang dapat ditambahkan.

Pemisahan konsep:

- User adalah identitas untuk autentikasi.
- Employee adalah data kepegawaian.
- User dapat dihubungkan dengan Employee, tetapi keduanya bukan entitas yang sama.

5.3 Role-Based Access Control

Model utama:

- User.
- Role.
- Permission.
- UserRole.
- RolePermission.

Permission harus mendukung granularitas:

"module.resource.action"

Contoh:

- "hrm.employee.view"
- "hrm.employee.create"
- "hrm.employee.update"
- "hrm.employee.delete"
- "finance.journal.post"
- "crm.customer.export"
- "inventory.stock.transfer"

Role merupakan kumpulan permission.

Contoh role:

- Super Administrator.
- Organization Administrator.
- HR Manager.
- Finance Manager.
- Accountant.
- Sales Manager.
- Sales Staff.
- Project Manager.
- Employee.
- Auditor.

Role bawaan dapat disediakan, tetapi harus dapat dikonfigurasi.

5.4 Data Scope Authorization

RBAC harus dilengkapi pembatasan cakupan data.

Contoh:

- Global organization scope.
- Business unit scope.
- Branch scope.
- Department scope.
- Own record scope.
- Assigned project scope.

Seseorang dapat memiliki permission untuk melihat data karyawan, tetapi hanya pada cabang tertentu.

Otorisasi harus memeriksa:

1. Apakah user memiliki permission?
2. Apakah user memiliki akses pada record tersebut?
3. Apakah record berada dalam cakupan organisasinya?
4. Apakah kondisi workflow mengizinkan tindakan tersebut?

5.5 Workflow Engine

Workflow digunakan untuk mengatur proses persetujuan.

Fitur:

- Workflow definition.
- Workflow step.
- Approval level.
- Approver assignment.
- Conditional routing.
- Delegation.
- Rejection.
- Revision.
- Escalation.
- Approval history.

Contoh penggunaan:

- Leave approval.
- Purchase approval.
- Expense approval.
- Sales discount approval.
- Journal approval.
- Project budget approval.

Workflow harus memiliki riwayat yang tidak dapat diubah secara sembarangan.

5.6 Notification Center

Channel:

- In-app notification.
- Email notification.
- Webhook notification.

Fitur:

- Notification template.
- User preferences.
- Read/unread state.
- Notification history.
- Delivery status.
- Retry mechanism.

5.7 Audit Log

Audit log mencatat aktivitas penting.

Data minimum:

- Actor.
- Action.
- Entity type.
- Entity ID.
- Timestamp.
- Request ID.
- IP address bila relevan.
- Before state.
- After state.
- Context.

Data sensitif seperti password, token, dan secret tidak boleh disimpan dalam audit log.

Audit log harus dibatasi aksesnya dan memiliki kebijakan retensi.

5.8 Document Numbering

Sistem nomor dokumen harus mendukung:

- Prefix.
- Suffix.
- Period.
- Sequence.
- Reset policy.
- Branch-specific sequence bila diperlukan.

Contoh:

- INV/2026/000001.
- PO/JKT/2026/000001.
- SO/2026/000001.

Sequence harus aman terhadap concurrent requests dan tidak menghasilkan nomor ganda.

5.9 Custom Fields

Custom fields dapat digunakan untuk menambahkan atribut tertentu pada modul yang membutuhkannya.

Jenis:

- Text.
- Number.
- Date.
- Boolean.
- Select.
- Multi-select.
- URL.

Custom fields tidak boleh menggantikan kolom inti yang dibutuhkan untuk relasi, transaksi, validasi, atau query kritis.

---

6. BUSINESS MODULE SPECIFICATIONS

6.1 HRM / HRIS

Tujuan

Mengelola seluruh siklus hidup karyawan.

Submodul

A. Employee Management

- Employee profile.
- Employee number.
- Personal information.
- Employment information.
- Position.
- Department.
- Branch.
- Manager.
- Employment status.
- Contract history.
- Document attachment.
- Emergency contact.

B. Recruitment

- Job vacancy.
- Application.
- Candidate profile.
- Interview.
- Evaluation.
- Hiring decision.
- Onboarding.

C. Attendance

- Attendance record.
- Clock-in.
- Clock-out.
- Shift.
- Work schedule.
- Overtime.
- Attendance correction.
- Attendance approval.

D. Leave Management

- Leave type.
- Leave balance.
- Leave request.
- Approval.
- Holiday calendar.
- Leave history.

E. Payroll

- Salary component.
- Allowance.
- Deduction.
- Payroll period.
- Payroll calculation.
- Payroll approval.
- Payslip.
- Payroll journal integration.

Payroll calculation harus mendukung konfigurasi kebijakan dan regulasi sesuai yurisdiksi implementasi.

F. Performance Management

- KPI.
- Performance review.
- Goal.
- Competency.
- Evaluation.
- Performance history.

G. Training

- Employee training record.
- Training assignment.
- Certification.
- Training history.

6.2 Finance and Accounting

Tujuan

Mengelola pencatatan keuangan dan transaksi akuntansi.

Submodul

A. General Ledger

- Chart of Accounts.
- Journal Entry.
- Journal Line.
- Posting.
- Reversal.
- General Ledger.
- Trial Balance.
- Period Closing.

B. Accounts Receivable

- Customer invoice.
- Payment.
- Outstanding balance.
- Credit note.
- Receivable aging.

C. Accounts Payable

- Vendor bill.
- Payment.
- Vendor credit.
- Payable aging.

D. Cash and Bank

- Cash account.
- Bank account.
- Bank transaction.
- Bank reconciliation.
- Cash movement.

E. Expense Management

- Expense claim.
- Expense category.
- Receipt.
- Approval.
- Reimbursement.

F. Budget Management

- Budget.
- Budget allocation.
- Actual versus budget.
- Variance.
- Budget approval.

G. Financial Reporting

- Balance sheet.
- Income statement.
- Cash flow statement.
- General ledger report.
- Trial balance.
- Receivable report.
- Payable report.

Accounting Rules

1. Setiap journal yang diposting harus balance.
2. Debit harus sama dengan kredit.
3. Journal yang telah diposting tidak boleh diedit secara langsung.
4. Koreksi dilakukan melalui reversal atau adjustment.
5. Periode akuntansi dapat dikunci.
6. Setiap transaksi memiliki referensi sumber.
7. Transaksi harus memiliki audit trail.
8. Nilai uang menggunakan tipe numerik presisi, bukan floating point.

6.3 CRM and Sales

Tujuan

Mengelola hubungan pelanggan dan proses penjualan.

Submodul

A. Lead Management

- Lead capture.
- Lead source.
- Lead assignment.
- Lead scoring.
- Lead qualification.
- Lead conversion.

B. Contact Management

- Contact profile.
- Customer profile.
- Organization profile.
- Contact history.
- Relationship mapping.

C. Sales Pipeline

- Pipeline.
- Sales stage.
- Opportunity.
- Probability.
- Expected closing date.
- Sales forecast.

D. Quotation

- Quotation.
- Quotation item.
- Discount.
- Tax.
- Validity.
- Approval.
- PDF generation.

E. Sales Order

- Sales order.
- Sales item.
- Delivery status.
- Order confirmation.
- Invoice integration.

F. Customer Interaction

- Call log.
- Meeting.
- Email log.
- Notes.
- Follow-up.
- Activity history.

6.4 Project Management

Tujuan

Mengelola proyek, pekerjaan, waktu, dan sumber daya.

Submodul

A. Project

- Project profile.
- Project code.
- Client.
- Project manager.
- Project member.
- Budget.
- Timeline.
- Status.

B. Task Management

- Task.
- Subtask.
- Assignee.
- Priority.
- Due date.
- Dependency.
- Checklist.
- Attachment.
- Comment.

C. Project Planning

- Milestone.
- Gantt data.
- Timeline.
- Resource allocation.
- Work breakdown structure.

D. Time Tracking

- Timesheet.
- Work log.
- Billable hours.
- Approval.
- Productivity report.

E. Project Finance

- Project budget.
- Project cost.
- Expense allocation.
- Revenue tracking.
- Profitability.

6.5 Collaboration

Tujuan

Menyediakan fasilitas komunikasi dan kolaborasi internal.

Submodul:

- Internal announcement.
- Discussion.
- Comments.
- Direct messaging.
- Group messaging.
- Calendar.
- Meeting.
- Shared documents.
- Activity feed.
- Mention.
- Notification.

Pesan dan dokumen harus memiliki pengaturan akses yang mengikuti konteks organisasi atau proyek.

6.6 Inventory Management

Tujuan

Mengelola barang, persediaan, gudang, dan pergerakan stok.

Submodul:

- Product.
- Product category.
- SKU.
- Unit of measure.
- Warehouse.
- Warehouse location.
- Stock balance.
- Stock movement.
- Stock transfer.
- Stock adjustment.
- Stock opname.
- Batch and lot.
- Serial number.
- Reorder level.
- Inventory valuation.

Aturan:

- Semua perubahan stok harus menghasilkan stock movement.
- Saldo stok tidak boleh diubah tanpa transaksi yang dapat ditelusuri.
- Transfer antar gudang harus memiliki proses pengiriman dan penerimaan.
- Kebijakan costing harus ditetapkan secara eksplisit, misalnya FIFO atau weighted average.
- Nilai persediaan harus dapat direkonsiliasi dengan accounting.

6.7 Point of Sale

Tujuan

Mengelola transaksi penjualan langsung.

Submodul:

- POS terminal.
- Cashier.
- Shift.
- Product catalog.
- Cart.
- Discount.
- Tax.
- Payment method.
- Sales transaction.
- Receipt.
- Return.
- Refund.
- Cash closing.
- Daily sales report.

Integrasi:

- Inventory.
- Product.
- Customer.
- Finance.
- Accounting.

Transaksi POS harus aman terhadap duplikasi request, terutama saat pembayaran dan pencatatan stok.

6.8 Learning Management System

Tujuan

Mengelola proses pembelajaran dan pelatihan.

Submodul:

- Course.
- Course category.
- Learning path.
- Lesson.
- Learning material.
- Enrollment.
- Assignment.
- Quiz.
- Assessment.
- Certification.
- Learning progress.
- Completion record.

Integrasi:

- HRM.
- Employee profile.
- Organization.
- Notification.

6.9 Analytics and Reporting

Tujuan

Menyediakan informasi operasional dan manajerial.

Kategori:

- Executive dashboard.
- HR dashboard.
- Finance dashboard.
- Sales dashboard.
- Project dashboard.
- Inventory dashboard.
- POS dashboard.
- Learning dashboard.

Fitur:

- KPI.
- Charts.
- Filters.
- Date range.
- Drill-down.
- Export CSV.
- Export XLSX.
- PDF reports.
- Scheduled reports.

Reporting harus menggunakan query yang terkontrol. Proses laporan berat dapat dijalankan melalui queue atau read model khusus.

---

7. DATABASE ARCHITECTURE

7.1 Prinsip Database

- PostgreSQL sebagai database utama.
- Relational model sebagai standar.
- Foreign key untuk integritas referensial.
- Database transaction untuk perubahan atomik.
- Migration untuk setiap perubahan skema.
- Index berdasarkan pola akses.
- Tidak menggunakan JSON sebagai pengganti relasi utama.
- Data transaksi memiliki referensi yang jelas.
- Soft delete tidak diterapkan secara otomatis pada semua tabel.

7.2 Konvensi Penamaan

Tabel:

- Lowercase.
- Snake case.
- Plural untuk tabel koleksi.

Contoh:

- "users"
- "employees"
- "departments"
- "sales_orders"
- "journal_entries"

Kolom:

- "snake_case"
- Foreign key menggunakan nama entitas ditambah "_id".

Contoh:

- "employee_id"
- "branch_id"
- "created_at"
- "updated_at"

7.3 Kolom Standar

Tabel operasional dapat menggunakan:

- "id"
- "created_at"
- "updated_at"
- "created_by"
- "updated_by"

Tabel yang mendukung penghapusan logis dapat menggunakan:

- "deleted_at"
- "deleted_by"

Tidak semua tabel wajib menggunakan seluruh kolom tersebut. Tabel log dan ledger memiliki kebutuhan khusus.

7.4 Core Database Entities

Identity

- users
- user_profiles
- user_sessions
- password_reset_tokens
- roles
- permissions
- role_permissions
- user_roles

Organization

- organizations
- business_units
- branches
- divisions
- departments
- teams
- positions
- organization_hierarchies

Workflow

- workflow_definitions
- workflow_versions
- workflow_steps
- workflow_instances
- workflow_approvals
- workflow_histories

Notification

- notifications
- notification_templates
- notification_preferences
- notification_deliveries

Audit

- audit_logs
- activity_logs

Documents

- files
- file_versions
- document_categories
- document_relations

7.5 Relational Integrity

Setiap relasi penting harus memiliki:

- Foreign key.
- Referential action yang tepat.
- Unique constraint bila diperlukan.
- Index untuk query yang sering digunakan.

Penghapusan record master tidak boleh merusak transaksi historis.

7.6 Financial Data Model

Entitas utama:

- chart_of_accounts
- accounting_periods
- journal_entries
- journal_lines
- fiscal_years
- bank_accounts
- payments
- invoices
- bills

Journal line minimal memiliki:

- journal_entry_id
- account_id
- debit
- credit
- currency_code bila multi-currency digunakan
- description
- dimensions atau referensi analitis bila dibutuhkan

Nilai debit dan kredit harus menggunakan numeric dengan presisi yang ditentukan.

7.7 Inventory Data Model

Entitas utama:

- products
- product_categories
- units_of_measure
- warehouses
- warehouse_locations
- stock_movements
- stock_movement_lines
- stock_balances
- stock_transfers
- stock_adjustments
- inventory_valuations

Stock movement menjadi sumber riwayat perubahan persediaan.

Stock balance dapat digunakan sebagai read model atau saldo terhitung yang diperbarui secara konsisten.

7.8 Data Lifecycle

Data dikategorikan menjadi:

1. Master data.
2. Transactional data.
3. Reference data.
4. Historical data.
5. Audit data.
6. Temporary data.

Setiap kategori memiliki aturan retensi, perubahan, penghapusan, dan backup masing-masing.

---

8. API ARCHITECTURE

8.1 API Standard

Base path:

"/api/v1"

Format:

- HTTP.
- JSON.
- UTF-8.
- REST conventions.

Contoh:

"GET /api/v1/employees"

"POST /api/v1/employees"

"GET /api/v1/employees/{id}"

"PATCH /api/v1/employees/{id}"

"DELETE /api/v1/employees/{id}"

8.2 Response Standard

Success response:

{
  "success": true,
  "data": {},
  "meta": {},
  "message": "Operation successful"
}

Error response:

{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed",
    "details": {}
  }
}

Response harus konsisten, tetapi tetap mengikuti HTTP status code yang tepat.

8.3 HTTP Status

- 200: Success.
- 201: Created.
- 202: Accepted for asynchronous processing.
- 204: Success without response body.
- 400: Bad request.
- 401: Unauthenticated.
- 403: Forbidden.
- 404: Not found.
- 409: Conflict.
- 422: Validation error.
- 429: Rate limited.
- 500: Internal server error.

8.4 Pagination

Gunakan pagination untuk collection endpoint.

Parameter:

- "page"
- "per_page"
- "sort"
- "direction"
- "search"
- Filter khusus modul.

Response metadata:

- Current page.
- Per page.
- Total.
- Last page.

Untuk data sangat besar atau perubahan data yang sering, cursor pagination dapat dipertimbangkan.

8.5 API Versioning

Perubahan yang memutus kompatibilitas harus menggunakan versi API baru atau mekanisme migrasi yang terdokumentasi.

8.6 Idempotency

Operasi yang berisiko menghasilkan transaksi ganda harus mendukung idempotency key.

Contoh:

- Payment.
- POS checkout.
- Journal posting.
- Stock transfer.
- External webhook processing.

---

9. AUTHENTICATION AND AUTHORIZATION

9.1 Authentication

Metode awal:

- Username atau email dan password.
- Secure session atau token-based authentication sesuai pola deployment.
- Password hashing dengan algoritma yang direkomendasikan framework.
- Password reset.
- Session revocation.
- Login throttling.

Untuk aplikasi web first-party, secure cookie-based session merupakan pilihan awal yang baik apabila frontend dan backend dapat dioperasikan dalam konfigurasi domain yang sesuai.

9.2 Authorization

Authorization dilakukan pada backend.

Setiap endpoint yang memerlukan akses harus:

1. Memastikan user terautentikasi.
2. Memeriksa permission.
3. Memeriksa data scope.
4. Memeriksa business state.
5. Mencatat aktivitas sensitif.

9.3 Session Security

- HttpOnly cookie.
- Secure cookie pada HTTPS.
- SameSite policy.
- Session expiration.
- Session invalidation.
- CSRF protection untuk cookie-based authentication.
- Rate limiting pada login.

---

10. SECURITY SPECIFICATION

10.1 Application Security

Sistem harus melindungi dari:

- SQL injection.
- Cross-site scripting.
- Cross-site request forgery.
- Broken access control.
- Insecure direct object reference.
- File upload abuse.
- Authentication attacks.
- Session hijacking.
- Mass assignment.
- Sensitive data exposure.

10.2 Data Protection

- HTTPS untuk seluruh akses produksi.
- Secret tidak disimpan dalam source code.
- Password tidak boleh disimpan dalam bentuk plaintext.
- Sensitive fields harus dilindungi sesuai tingkat risikonya.
- Backup harus dilindungi.
- Log tidak boleh membocorkan credential atau token.
- File sensitif tidak boleh tersedia melalui URL publik permanen.

10.3 Administrative Security

Fitur:

- Admin activity log.
- Login history.
- Active session review.
- Account suspension.
- Password policy.
- Optional MFA.
- Backup access control.
- Configuration access control.

10.4 Security Operations

Harus tersedia:

- Dependency update process.
- Security patch process.
- Vulnerability review.
- Access review.
- Log review.
- Backup verification.
- Incident response procedure.

---

11. INTEGRATION ARCHITECTURE

11.1 Internal Integration

Integrasi internal menggunakan:

- Application service.
- Domain event.
- Shared reference contracts.
- Query interface.

Contoh:

- HRM mengirim payroll result ke Finance.
- Sales membuat invoice melalui Finance.
- POS membuat stock movement melalui Inventory.
- Project menggunakan employee reference dari HRM.
- LMS menghubungkan enrollment dengan Employee.

11.2 External Integration

Sistem dirancang agar dapat diintegrasikan dengan:

- Email SMTP.
- Payment gateway.
- Banking API.
- Tax service.
- Cloud storage.
- Messaging provider.
- Accounting export.
- External HR systems.
- Webhook consumer.

Integrasi tertentu baru dapat diaktifkan setelah penyedia dan kebutuhan bisnis ditetapkan.

11.3 Webhook

Fitur:

- Webhook endpoint registration.
- Event subscription.
- Signature verification.
- Retry.
- Delivery log.
- Timeout.
- Idempotent handling.

11.4 Import and Export

Fitur:

- CSV import.
- XLSX import.
- CSV export.
- XLSX export.
- PDF export.

Import harus memiliki:

- Template.
- Field mapping.
- Validation.
- Preview.
- Error report.
- Duplicate detection.
- Transaction strategy.

---

12. FRONTEND ARCHITECTURE

12.1 Application Layout

Struktur UI:

- Authentication layout.
- Main application layout.
- Dashboard layout.
- Module layout.
- Administrative layout.

Komponen utama:

- Sidebar.
- Top navigation.
- Breadcrumb.
- Page header.
- Search.
- Notification center.
- User menu.
- Content area.

12.2 Component Architecture

Kategori:

- Primitive components.
- Form components.
- Data display components.
- Navigation components.
- Feedback components.
- Business components.
- Module-specific components.

Contoh:

- Button.
- Input.
- Select.
- Date picker.
- Data table.
- Modal.
- Drawer.
- Dropdown.
- Tabs.
- Badge.
- Chart.
- File uploader.

12.3 Feature-Based Organization

Frontend disusun berdasarkan fitur dan domain, bukan hanya berdasarkan jenis file.

Setiap feature dapat memiliki:

- Components.
- Hooks.
- Services.
- Types.
- Schemas.
- Pages atau routes.
- Tests.

12.4 State Management

Pemisahan:

- Server state.
- Local UI state.
- Form state.
- Authentication state.
- Global preferences.

Hindari menyimpan data server yang sama di banyak state store tanpa alasan yang jelas.

12.5 User Experience Standards

- Responsive.
- Consistent layout.
- Clear validation.
- Loading state.
- Empty state.
- Error state.
- Confirmation for destructive actions.
- Keyboard accessibility.
- Consistent date, number, and currency formatting.

---

13. BACKEND PROJECT STRUCTURE

Struktur konseptual:

backend/
├── app/
│   ├── Modules/
│   │   ├── Core/
│   │   │   ├── Organization/
│   │   │   ├── Identity/
│   │   │   ├── AccessControl/
│   │   │   ├── Workflow/
│   │   │   ├── Notification/
│   │   │   ├── Audit/
│   │   │   └── Documents/
│   │   ├── HRM/
│   │   ├── Finance/
│   │   ├── CRM/
│   │   ├── Sales/
│   │   ├── Project/
│   │   ├── Collaboration/
│   │   ├── Inventory/
│   │   ├── POS/
│   │   ├── LMS/
│   │   └── Analytics/
│   ├── Shared/
│   └── Providers/
├── bootstrap/
├── config/
├── database/
│   ├── migrations/
│   ├── seeders/
│   └── factories/
├── routes/
├── storage/
└── tests/

Setiap modul dapat memiliki struktur:

ModuleName/
├── Domain/
│   ├── Entities/
│   ├── ValueObjects/
│   ├── Events/
│   ├── Services/
│   └── Exceptions/
├── Application/
│   ├── Commands/
│   ├── Queries/
│   ├── DTOs/
│   └── Services/
├── Infrastructure/
│   ├── Persistence/
│   ├── Repositories/
│   └── Integrations/
├── Presentation/
│   └── Http/
│       ├── Controllers/
│       ├── Requests/
│       └── Resources/
└── Tests/

Struktur ini merupakan logical architecture. Implementasi Laravel dapat menggunakan konvensi framework sepanjang batas domain tetap terjaga.

---

14. FRONTEND PROJECT STRUCTURE

Struktur konseptual:

frontend/
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   ├── (dashboard)/
│   │   └── admin/
│   ├── components/
│   │   ├── ui/
│   │   ├── forms/
│   │   ├── tables/
│   │   └── layout/
│   ├── features/
│   │   ├── organization/
│   │   ├── hrm/
│   │   ├── finance/
│   │   ├── crm/
│   │   ├── sales/
│   │   ├── project/
│   │   ├── collaboration/
│   │   ├── inventory/
│   │   ├── pos/
│   │   ├── lms/
│   │   └── analytics/
│   ├── lib/
│   ├── hooks/
│   ├── services/
│   ├── types/
│   └── styles/
├── public/
└── tests/

---

15. DEPLOYMENT ARCHITECTURE

15.1 Deployment Components

Production deployment minimal:

1. NGINX reverse proxy.
2. Next.js application.
3. Laravel application.
4. PostgreSQL.
5. Redis.
6. Queue worker.
7. Scheduler.
8. Persistent file storage.

15.2 Container Architecture

Docker Compose digunakan untuk mengelola service.

Service konseptual:

- "nginx"
- "frontend"
- "backend"
- "queue-worker"
- "scheduler"
- "postgres"
- "redis"

Data PostgreSQL dan file upload harus menggunakan persistent volumes.

15.3 Environment Configuration

Konfigurasi dipisahkan menjadi:

- Development.
- Testing.
- Staging.
- Production.

Secret disimpan melalui environment configuration atau secret management yang sesuai.

File environment tidak boleh masuk ke repository.

15.4 Installation Process

Proses instalasi harus mencakup:

1. Pemeriksaan kebutuhan server.
2. Pemeriksaan Docker dan dependency.
3. Pembuatan environment configuration.
4. Koneksi database.
5. Pembuatan database schema.
6. Migration.
7. Initial administrator.
8. Initial organization.
9. Storage configuration.
10. SMTP configuration.
11. Application key dan secret.
12. Health check.
13. Installation lock.

Setelah instalasi berhasil, installer harus dinonaktifkan atau dikunci agar tidak dapat dijalankan ulang tanpa prosedur administratif yang aman.

15.5 Backup and Restore

Backup mencakup:

- PostgreSQL.
- Uploaded files.
- Application configuration.
- Necessary encryption keys dan secrets melalui prosedur penyimpanan aman.

Jenis backup:

- Full backup.
- Incremental backup bila didukung.
- Database backup.
- File backup.

Restore harus memiliki prosedur terdokumentasi dan diuji secara berkala.

---

16. SYSTEM ADMINISTRATION

16.1 System Dashboard

Informasi:

- Application version.
- Database connection.
- Queue status.
- Storage usage.
- Disk capacity.
- Worker status.
- Scheduler status.
- Recent errors.
- Last backup.

16.2 System Configuration

Fitur:

- Organization settings.
- Application settings.
- Locale.
- Timezone.
- Currency.
- Email.
- Storage.
- Module settings.
- Notification settings.

16.3 Log Management

Kategori:

- Application log.
- Authentication log.
- Audit log.
- Queue log.
- Integration log.
- System health log.

Log harus memiliki retensi dan rotasi.

16.4 Update and Migration

Prosedur update:

1. Validate current version.
2. Create backup.
3. Validate package.
4. Run maintenance mode bila diperlukan.
5. Apply migration.
6. Update application.
7. Clear cache.
8. Restart worker.
9. Run health checks.
10. Record update result.

Rollback tidak boleh mengandalkan pembalikan migration secara otomatis saja. Prosedur pemulihan harus mempertimbangkan konsistensi data dan kompatibilitas versi.

---

17. NON-FUNCTIONAL REQUIREMENTS

17.1 Performance

Target awal untuk baseline pengujian, bukan jaminan tanpa benchmark:

- API read sederhana: p95 di bawah 500 ms pada lingkungan uji yang ditentukan.
- API write sederhana: p95 di bawah 800 ms pada lingkungan uji yang ditentukan.
- Dashboard: target tampilan awal di bawah 3 detik untuk data dan infrastruktur yang ditentukan.
- Laporan besar: diproses secara asynchronous.
- Pagination wajib pada daftar data besar.

Target final harus ditetapkan berdasarkan profil server, volume data, concurrency, dan workload aktual.

17.2 Scalability

Skala awal dilakukan secara vertical:

- CPU.
- RAM.
- Storage.
- Database tuning.

Kemudian dapat dilakukan:

- Pemisahan worker.
- Database read replica.
- External object storage.
- Dedicated reporting workload.

Tidak perlu langsung menerapkan microservices.

17.3 Availability

Kebutuhan:

- Health check.
- Process restart policy.
- Database backup.
- Queue monitoring.
- Error logging.
- Restore procedure.

Target availability dan recovery time harus ditentukan berdasarkan kebutuhan operasional organisasi.

17.4 Maintainability

- Modular code.
- Consistent naming.
- Automated tests.
- API documentation.
- Migration history.
- Dependency management.
- Code review.
- Version control.

17.5 Compatibility

Sistem harus memiliki matriks kompatibilitas untuk:

- Browser.
- Linux distribution.
- Docker.
- PHP.
- Laravel.
- Node.js.
- Next.js.
- PostgreSQL.
- Redis.

Versi dependency harus dikunci dan diperbarui secara terencana.

---

18. TESTING STRATEGY

18.1 Unit Testing

Menguji:

- Domain rules.
- Calculations.
- Validation rules.
- Permission rules.
- Value objects.
- Business policies.

18.2 Integration Testing

Menguji:

- Database.
- Repository.
- Queue.
- Storage.
- Email.
- External API.
- Module integration.

18.3 API Testing

Menguji:

- Authentication.
- Authorization.
- Validation.
- Response format.
- Pagination.
- Filtering.
- Error handling.
- Idempotency.

18.4 End-to-End Testing

Menguji proses bisnis dari sudut pandang pengguna.

Contoh:

- Login.
- Membuat employee.
- Mengajukan cuti.
- Menyetujui cuti.
- Membuat sales order.
- Membuat invoice.
- Menerima pembayaran.
- Membuat journal.
- Menjalankan closing.

18.5 Security Testing

- Permission bypass.
- IDOR.
- SQL injection.
- XSS.
- CSRF.
- Session handling.
- File upload validation.
- Rate limiting.
- Sensitive data exposure.

18.6 Acceptance Criteria

Sebuah modul dinyatakan siap apabila:

1. Seluruh use case prioritas telah tersedia.
2. Business rules berjalan sesuai spesifikasi.
3. Authorization diuji.
4. Database constraints berjalan.
5. API terdokumentasi.
6. Unit dan integration tests lulus.
7. Audit trail tersedia untuk tindakan kritis.
8. Error handling tersedia.
9. Dokumentasi penggunaan tersedia.
10. Tidak terdapat defect kritis yang belum diselesaikan.

---

19. DEVELOPMENT STANDARDS

19.1 Coding Standards

- Consistent naming.
- Type safety.
- Clear separation of concerns.
- Dependency injection.
- Small and focused functions.
- Avoid duplicated business logic.
- Avoid direct cross-module database writes.
- Avoid unnecessary abstractions.

19.2 Git Workflow

Branch:

- "main"
- "develop"
- "feature/*"
- "bugfix/*"
- "hotfix/*"

Setiap perubahan harus melalui:

- Pull request.
- Code review.
- Automated tests.
- Migration review.
- Security review untuk perubahan sensitif.

19.3 Documentation

Setiap modul wajib memiliki:

- Module overview.
- Functional scope.
- Domain model.
- Database schema.
- API endpoints.
- Permission list.
- Workflow.
- Integration dependencies.
- Test cases.

19.4 Definition of Done

Fitur selesai apabila:

- Requirement terpenuhi.
- UI tersedia.
- API tersedia.
- Validasi tersedia.
- Permission tersedia.
- Audit tersedia jika relevan.
- Tests lulus.
- Dokumentasi diperbarui.
- Tidak ada error kritis.
- Perubahan database memiliki migration.

---

20. IMPLEMENTATION ROADMAP

Urutan pengembangan yang direkomendasikan:

Phase 1 — Architecture Foundation

- Repository setup.
- Backend setup.
- Frontend setup.
- Docker environment.
- Database connection.
- Logging.
- Error handling.
- API conventions.
- CI pipeline.

Phase 2 — Core System

- Organization.
- Identity.
- Authentication.
- RBAC.
- Data scope.
- Settings.
- Audit log.
- Notification.
- File storage.
- Document numbering.

Phase 3 — Workflow and Shared Services

- Workflow engine.
- Approval engine.
- Activity history.
- Import/export.
- Scheduler.
- Queue infrastructure.

Phase 4 — Business Modules

Urutan awal yang disarankan:

1. HRM.
2. Finance and Accounting.
3. CRM.
4. Sales.
5. Inventory.
6. Project Management.
7. POS.
8. Collaboration.
9. LMS.
10. Analytics.

Urutan dapat berubah berdasarkan kebutuhan bisnis dan dependensi antarmodul.

Phase 5 — Integration

- Internal module integration.
- External API.
- Webhook.
- Email.
- Storage integration.
- Data import.

Phase 6 — Hardening and Production

- Performance testing.
- Security testing.
- Backup testing.
- Restore testing.
- Deployment automation.
- Monitoring.
- Operational documentation.

---

21. ARCHITECTURAL DECISIONS

Keputusan arsitektur baseline:

ID| Keputusan| Status
ADR-001| Self-hosted deployment| Baseline
ADR-002| Single-tenant installation| Baseline
ADR-003| Modular monolith| Baseline
ADR-004| Laravel backend| Proposed
ADR-005| Next.js frontend| Proposed
ADR-006| PostgreSQL database| Proposed
ADR-007| Redis cache and queue| Proposed
ADR-008| REST API| Baseline
ADR-009| Docker Compose deployment| Proposed
ADR-010| RBAC with data scope| Baseline
ADR-011| Domain-based module boundaries| Baseline
ADR-012| Event-driven internal integration| Baseline

Setiap keputusan penting yang berubah harus dicatat sebagai Architecture Decision Record (ADR), termasuk alasan perubahan dan dampaknya.

---

22. OPEN TECHNICAL DECISIONS

Bagian ini mencatat hal yang belum boleh dianggap final sebelum desain rinci.

1. UUID atau ULID sebagai standar primary key.
2. Session-based authentication atau token strategy untuk seluruh client.
3. Apakah multi-currency dibutuhkan sejak versi pertama.
4. Kebijakan costing inventory.
5. Apakah payroll menjadi modul penuh atau integrasi awal.
6. Apakah collaboration membutuhkan real-time messaging sejak versi pertama.
7. Apakah file storage awal menggunakan local storage saja atau langsung S3-compatible.
8. Apakah search engine khusus diperlukan.
9. Apakah reporting memakai database utama atau read model terpisah.
10. Apakah organisasi memerlukan multi-company accounting dalam satu instalasi.
11. Apakah offline POS menjadi kebutuhan.
12. Apakah custom workflow harus mendukung conditional branching kompleks.
13. Target jumlah pengguna bersamaan dan volume data.
14. Kebijakan backup, retensi, RPO, dan RTO.
15. Versi final framework dan runtime ketika implementasi dimulai.

---

23. FINAL ARCHITECTURAL BASELINE

Sistem akan dibangun dengan fondasi:

Frontend

- Next.js
- React
- TypeScript
- Reusable component architecture

Backend

- Laravel
- Modular monolith
- REST API
- Application and domain services

Database

- PostgreSQL
- Relational schema
- Transactional integrity

Infrastructure

- Redis
- Docker Compose
- NGINX
- Linux
- Persistent storage

Core capabilities

- Organization management.
- Identity and access control.
- Data scope authorization.
- Workflow engine.
- Notification center.
- Audit log.
- Document management.
- System administration.

Business capabilities

- HRM.
- Finance and Accounting.
- CRM.
- Sales.
- Project Management.
- Collaboration.
- Inventory.
- POS.
- LMS.
- Analytics.

Architectural direction

Satu instalasi, satu aplikasi, satu database utama, banyak modul bisnis dengan domain yang terpisah secara logis, serta satu standar keamanan dan integrasi.

Dokumen ini merupakan baseline arsitektur tingkat induk. Setiap modul tetap memerlukan spesifikasi rinci tersendiri sebelum implementasi agar business rules, database schema, API contract, workflow, permission matrix, dan acceptance criteria tidak hanya diasumsikan dari nama fitur.
