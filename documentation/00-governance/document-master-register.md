# Document Master Register

| Atribut | Nilai |
|---|---|
| Document Code | GOV-001 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Documentation Manager |
| Related Documents | `documentation/00-governance/documentation-standard.md`, `documentation/00-governance/documentation-plan.md`, `docs/MTS.md` |

## Tujuan

Register ini mencatat dokumen yang diwajibkan oleh `AGENT.md`, pemilik domain, ketergantungan utama, serta status penyusunannya. MTS v1.0 berstatus Draft Baseline; dokumen turunan tetap DRAFT sampai ada persetujuan manusia. Status isi dokumen tidak menyatakan persetujuan atas keputusan arsitektur MTS yang berstatus Proposed.

## Cakupan dan sumber kebenaran

Mencakup DOC-001 sampai DOC-040 dan document pack modul. Sumber arsitektur utama adalah [`docs/MTS.md`](../../docs/MTS.md); instruksi dokumentasi adalah [`AGENT.md`](../../AGENT.md). Detail yang belum diputuskan tetap TBD, TBC, atau ADR Required sebagaimana relevan. Register ini bukan bukti persetujuan kebutuhan bisnis.

## Status

- **PLANNED**: terdaftar, belum disusun.
- **DRAFT**: draf tersedia, belum ditinjau atau disetujui.
- **IN REVIEW**: sedang ditinjau oleh pemilik yang ditetapkan.
- **APPROVED**: persetujuan manusia tercatat.
- **DEPRECATED**: digantikan atau tidak berlaku.

Semua dokumen di register berstatus PLANNED kecuali dokumen tata kelola awal yang dibuat bersama register ini: GOV-001, GOV-002, GOV-003, dan GOV-004 berstatus DRAFT.

## Master Register

### Requirements & Product

| Kode | Dokumen | Lokasi | Prasyarat utama | Status |
|---|---|---|---|---|
| DOC-001 | Business Requirements Specification (BRS) | `documentation/01-requirements/brs.md` | MTS, glossary | DRAFT |
| DOC-002 | Software Requirements Specification (SRS) | `documentation/01-requirements/srs.md` | DOC-001 | DRAFT |
| DOC-003 | Functional Requirements Specification (FRS) | `documentation/01-requirements/frs.md` | DOC-001, DOC-002 | DRAFT |
| DOC-004 | Use Case Specification (UCS) | `documentation/01-requirements/use-case-specification.md` | DOC-002, DOC-003 | DRAFT |
| DOC-005 | Business Process Specification (BPS) | `documentation/01-requirements/business-process-specification.md` | DOC-001, DOC-003 | DRAFT |
| DOC-006 | Business Rules Specification (BRS-RULES) | `documentation/01-requirements/business-rules-specification.md` | DOC-003, DOC-005 | DRAFT |

### Architecture

| Kode | Dokumen | Lokasi | Prasyarat utama | Status |
|---|---|---|---|---|
| DOC-007 | Software Architecture Description (SAD) | `documentation/02-architecture/sad.md` | MTS, DOC-002 | DRAFT |
| DOC-008 | System Architecture Specification (SAS) | `documentation/02-architecture/sas.md` | DOC-007 | DRAFT |
| DOC-009 | Architecture Decision Records (ADR) | `documentation/02-architecture/adr/` | MTS, DOC-007 | DRAFT |
| DOC-010 | Module Architecture Specification (MAS) | `documentation/02-architecture/module-architecture/` | DOC-007, DOC-012 | DRAFT |
| DOC-011 | Integration Architecture Specification (IAS) | `documentation/02-architecture/integration-architecture.md` | DOC-007, DOC-010 | DRAFT |
| DOC-012 | Module Dependency Matrix (MDM) | `documentation/02-architecture/module-dependency-matrix.md` | DOC-010, DOC-011 | DRAFT |

### Data & Database

| Kode | Dokumen | Lokasi | Prasyarat utama | Status |
|---|---|---|---|---|
| DOC-013 | Database Design Specification (DDS) | `documentation/03-data/database-design.md` | DOC-010, DOC-012 | DRAFT |
| DOC-014 | Entity Relationship Specification (ERS) | `documentation/03-data/entity-relationship.md` | DOC-013 | DRAFT |
| DOC-015 | Data Dictionary (DD) | `documentation/03-data/data-dictionary.md` | DOC-013, DOC-014 | DRAFT |
| DOC-016 | Data Governance Specification (DGS) | `documentation/03-data/data-governance.md` | DOC-001, DOC-013 | DRAFT |
| DOC-017 | Data Migration Specification (DMS) | `documentation/03-data/data-migration.md` | DOC-013, DOC-016 | DRAFT |

### Backend & API

| Kode | Dokumen | Lokasi | Prasyarat utama | Status |
|---|---|---|---|---|
| DOC-018 | Backend Technical Specification (BTS) | `documentation/04-backend/backend-technical-specification.md` | DOC-007, DOC-010, DOC-013 | DRAFT |
| DOC-019 | API Specification (API-SPEC) | `documentation/04-backend/api/` | DOC-018, DOC-015, DOC-021 | DRAFT |
| DOC-020 | Event & Queue Specification (EQS) | `documentation/04-backend/event-queue-specification.md` | DOC-011, DOC-018 | DRAFT |
| DOC-021 | Authentication & Authorization Specification (AAS) | `documentation/04-backend/authentication-authorization.md` | DOC-002, DOC-007 | DRAFT |
| DOC-022 | Workflow Engine Specification (WES) | `documentation/04-backend/workflow-engine.md` | DOC-005, DOC-006, DOC-018 | DRAFT |

### Frontend & UX

| Kode | Dokumen | Lokasi | Prasyarat utama | Status |
|---|---|---|---|---|
| DOC-023 | User Interface & User Experience Specification (UIUX) | `documentation/05-frontend/ui-ux-specification.md` | DOC-002, DOC-003, DOC-026 | DRAFT |
| DOC-024 | Frontend Technical Specification (FTS) | `documentation/05-frontend/frontend-technical-specification.md` | DOC-007, DOC-023, DOC-025 | DRAFT |
| DOC-025 | Design System Specification (DSS) | `documentation/05-frontend/design-system.md` | DOC-023 | DRAFT |
| DOC-026 | Navigation & Information Architecture (IA) | `documentation/05-frontend/information-architecture.md` | DOC-003, DOC-010 | DRAFT |

### Security & Infrastructure

| Kode | Dokumen | Lokasi | Prasyarat utama | Status |
|---|---|---|---|---|
| DOC-027 | Security Technical Specification (STS) | `documentation/06-security-infrastructure/security-specification.md` | DOC-007, DOC-021 | DRAFT |
| DOC-028 | Infrastructure & Deployment Specification (IDS) | `documentation/06-security-infrastructure/infrastructure-deployment.md` | DOC-007, DOC-018, DOC-024 | DRAFT |
| DOC-029 | Backup & Disaster Recovery Specification (BDRS) | `documentation/06-security-infrastructure/backup-disaster-recovery.md` | DOC-013, DOC-028 | DRAFT |

### Quality Assurance & Testing

| Kode | Dokumen | Lokasi | Prasyarat utama | Status |
|---|---|---|---|---|
| DOC-030 | Software Testing Specification (STS-TEST) | `documentation/08-testing/software-testing-specification.md` | DOC-002, DOC-003, DOC-019 | DRAFT |
| DOC-031 | QA & Acceptance Specification (QAAS) | `documentation/08-testing/qa-acceptance-specification.md` | DOC-030, traceability matrix | DRAFT |

### Operations

| Kode | Dokumen | Lokasi | Prasyarat utama | Status |
|---|---|---|---|---|
| DOC-032 | Deployment Manual | `documentation/09-operations/deployment-manual.md` | DOC-028, DOC-029 | DRAFT |
| DOC-033 | System Administration Manual | `documentation/09-operations/system-administration-manual.md` | DOC-028, DOC-038 | DRAFT |
| DOC-034 | End User Manual | `documentation/09-operations/end-user-manual.md` | DOC-003, DOC-023, module packs | DRAFT |
| DOC-035 | API Integration Guide | `documentation/09-operations/api-integration-guide.md` | DOC-019, DOC-021 | DRAFT |
| DOC-036 | Troubleshooting Guide | `documentation/09-operations/troubleshooting-guide.md` | DOC-032, DOC-033, DOC-038 | DRAFT |
| DOC-037 | Release Management Specification | `documentation/09-operations/release-management.md` | DOC-028, DOC-030 | DRAFT |
| DOC-038 | Monitoring & Logging Specification | `documentation/09-operations/monitoring-logging.md` | DOC-018, DOC-028 | DRAFT |
| DOC-039 | Performance Benchmark Specification | `documentation/09-operations/performance-benchmark.md` | DOC-002, DOC-028, DOC-030 | DRAFT |
| DOC-040 | Code Convention & Development Standard | `documentation/09-operations/development-standard.md` | DOC-018, DOC-024 | DRAFT |

### Governance Support

| Kode | Dokumen | Lokasi | Prasyarat utama | Status |
|---|---|---|---|---|
| GOV-001 | Document Master Register (dokumen ini) | `documentation/00-governance/document-master-register.md` | MTS, AGENT | DRAFT |
| GOV-002 | Documentation Standard | `documentation/00-governance/documentation-standard.md` | MTS, AGENT | DRAFT |
| GOV-003 | Glossary | `documentation/00-governance/glossary.md` | MTS | DRAFT |
| GOV-004 | Documentation Plan & Dependencies | `documentation/00-governance/documentation-plan.md` | MTS, AGENT | DRAFT |
| GOV-005 | Traceability Matrix | `documentation/00-governance/traceability-matrix.md` | DOC-001–DOC-031 | DRAFT |
| GOV-006 | Test Traceability Matrix | `documentation/08-testing/test-traceability-matrix.md` | DOC-030, DOC-031 | DRAFT |

## Document Pack Modul

Setiap pack mencakup minimum 19 topik yang diwajibkan AGENT.md: overview, functional specification, business process, use cases, rules, architecture, database, ERD, data dictionary, API, events/queue, UI/UX, permissions, workflow, integration, validation, errors, tests, dan acceptance criteria. Detail yang merupakan aturan global harus merujuk dokumen global.

| Pack | Lokasi | Fokus kepemilikan | Status |
|---|---|---|---|
| Core System | `documentation/07-modules/core/` | Organization, Identity, RBAC, data scope, workflow, notification, audit, documents/files, numbering, custom fields, configuration, import/export | PLANNED |
| HRM / HRIS | `documentation/07-modules/hrm/` | Employee lifecycle, recruitment, attendance, leave, payroll, performance, training | PLANNED |
| Finance & Accounting | `documentation/07-modules/finance-accounting/` | Ledger, AR/AP, cash/bank, expense, budget, reporting | PLANNED |
| CRM | `documentation/07-modules/crm/` | Leads, contacts/customers, pipeline, interactions | PLANNED |
| Sales Management | `documentation/07-modules/sales/` | Quotation, sales order, discounts, tax, invoice handoff | PLANNED |
| Project Management | `documentation/07-modules/project-management/` | Projects, tasks, planning, time, project finance | PLANNED |
| Collaboration | `documentation/07-modules/collaboration/` | Announcements, discussions, messaging, calendar, shared documents, activity | PLANNED |
| Inventory Management | `documentation/07-modules/inventory/` | Products, warehouses, stock movement, transfer, adjustment, valuation | PLANNED |
| Point of Sale | `documentation/07-modules/pos/` | Terminal, shift, checkout, payment, receipt, returns/refunds, closing | PLANNED |
| Learning Management System | `documentation/07-modules/lms/` | Courses, enrollment, content, assessment, certification, progress | PLANNED |
| Analytics & Reporting | `documentation/07-modules/analytics-reporting/` | Dashboards, KPIs, query/read model, reports and exports | PLANNED |
| System Administration | `documentation/07-modules/system-administration/` | Installation, configuration, users/modules, queues, scheduler, health, backup, updates | PLANNED |

## Baseline dan Isu Terbuka

### Fakta baseline eksplisit di MTS

- Self-hosted, single-tenant per instalasi, banyak unit organisasi internal.
- Modular monolith, API-first REST dengan base path `/api/v1`.
- Arah teknologi: Laravel/PHP, Next.js/React/TypeScript, PostgreSQL, Redis, Docker Compose, NGINX, Linux.
- Antarmuka antarmodul melalui kontrak, application service, public interface, atau domain event; hindari akses tulis langsung lintas domain.
- Backend menjadi otoritas authorization; RBAC dilengkapi data scope.

### Keputusan berstatus Proposed atau terbuka

- ADR-004 sampai ADR-007 dan ADR-009 berstatus Proposed dalam tabel keputusan MTS; jangan perlakukan Proposed sebagai Approved.
- UUID vs ULID/strategi ID, session vs token untuk seluruh client, multi-currency, kebijakan costing, ruang lingkup payroll, real-time collaboration, storage awal, search engine, pelaporan/read model, multi-company accounting, offline POS, kompleksitas branching workflow, kapasitas, RPO/RTO, serta versi runtime final masih terbuka di MTS.
- Kebijakan pajak, payroll/yurisdiksi, mata uang, periode/closing rinci, retensi, ukuran workload, dan SLA operasional memerlukan konfirmasi pemilik bisnis/operasi.

### Konflik atau klarifikasi yang perlu ditinjau

1. MTS menyebut satu instalasi untuk satu organisasi, tetapi kebutuhan multi-company accounting dicatat sebagai keputusan terbuka. Jangan menyamakan entitas organisasi dengan company akuntansi sebelum keputusan dibuat.
2. MTS menyebut local atau S3-compatible storage, sementara pilihan awalnya terbuka. Dokumen turunan harus menjelaskan pilihan per deployment sebagai TBD.
3. MTS memberi target performa baseline yang bukan jaminan; profil hardware, data, concurrency, dan workload untuk benchmark belum ditetapkan.
4. Label Baseline/Proposed dalam tabel ADR MTS perlu dipertahankan apa adanya sampai status disepakati melalui ADR.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Register awal DOC-001–DOC-040, governance, dan 12 document pack berdasarkan MTS v1.0 | Documentation Agent |
