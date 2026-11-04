# Analytics & Reporting Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-ANALYTICS |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Data/Reporting Owner |
| Related Documents | `../../01-requirements/frs.md`, `../../02-architecture/integration-architecture.md`, `../../03-data/data-governance.md`, `../../08-testing/software-testing-specification.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Provides executive and domain dashboards, KPI, filters/date range, drill-down, CSV/XLSX/PDF export and scheduled reports. Source domains remain system of record.

## 2. Functional Specification

HR, Finance, Sales, Project, Inventory, POS and Learning dashboards; KPI/chart/filter/date range/drill-down; report generation/export; scheduled reports where configured. MTS does not define KPI formula/owner.

## 3. Business Process

Authorized user requests dashboard/report → validate dataset permission/scope/filter → execute controlled query or queue heavy report → store transient/result artifact per policy → recheck authorization on retrieval/export → log request/outcome as required.

## 4. Use Cases

View dashboard; filter period; drill down to permitted source record; export CSV/XLSX/PDF; schedule report. Actor manager/analyst; report recipients and scheduled delivery rules TBD.

## 5. Business Rules

Use controlled queries; large reports may be asynchronous or separate read model. Respect domain authorization and data scope in aggregate and drill-down; exports must not bypass access. Definitions and freshness of KPIs are TBD.

## 6. Module Architecture

Analytics owns report definitions/read models/cache/artifacts, not source transactions. Consume controlled query/data contracts from modules. Direct unrestricted cross-module joins are not assumed. Reporting DB vs primary/read model is open ADR.

## 7. Database Design

Potential report definitions, schedules, run statuses, filter presets, export metadata, read model tables/materialized views. Source business tables stay module-owned. Schema/cache/retention, artifact storage and scheduler ownership TBD.

## 8. Entity Relationship Diagram

Conceptual: Report definition has schedule/config; run references definition/requester/time/status and generated artifact; dashboard aggregates data products from domain modules. Exact data lineage and read-model relation TBD.

## 9. Data Dictionary

See DOC-015; report ID, owner, dataset/version, filter, requested scope, run status, generated time, artifact pointer/expiry, KPI definition/version. Classification and retention TBD.

## 10. API Specification

Candidate `/api/v1/reports`, `/reports/{id}/runs`, `/report-runs/{id}`, `/exports/{id}`. Authorization applies at request, query, drilldown and retrieval. Async 202/status, pagination, filters, errors and download expiry contract DOC-019.

## 11. Event & Queue Specification

Report requested/completed/failed; heavy report and scheduled jobs use queue per MTS. Job records status, correlation, retry safety and artifact cleanup; avoid duplicate scheduled delivery. DOC-020.

## 12. UI/UX Specification

Dashboard cards/charts, filter controls, drill-down table, export and report run history; show as-of/freshness timestamp and queued status. Accessible data alternatives and empty/error/loading states per DOC-023. KPI definitions visible after approved.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| View dashboard | `analytics.dashboard.view` | Role + module data scope |
| Run report | `analytics.report.run` | Dataset-specific authorization |
| Export | `analytics.report.export` | Field/record scope |
| Manage/schedule | `analytics.report.manage/schedule` | Reporting admin/owner |

No report permission should grant source record access automatically; final catalog TBD.

## 14. Workflow Specification

Report scheduling does not inherently require approval. Sensitive/large exports may require policy/workflow if owner decides; no threshold assumed. Engine reference DOC-022.

## 15. Integration Specification

Read/query contracts from HRM, Finance, CRM, Sales, Project, Inventory, POS, LMS; Core identity/scope/audit; queue and storage. Define lineage, freshness, calculation ownership, reconciliation and permission propagation per source.

## 16. Validation Rules

Allowlist dataset/columns/filters/sort; validate date ranges, page/export limits, requester scope, report status and artifact expiry. Avoid arbitrary SQL input. Exact limits and aggregation rules TBD.

## 17. Error Handling

403 dataset/scope denied; 422 unsupported filters/range; 409 stale/duplicate run where applicable; 202 queued; failed job reports sanitized reason and support correlation. Do not reveal hidden rows through counts/drill-down.

## 18. Test Specification

Scope-safe aggregate/drilldown/export, filter injection, KPI calculation/reconciliation, async retry/dedup, artifact access/expiry, large report performance, scheduled report recipients, data freshness and source-contract changes.

## 19. Acceptance Criteria

KPI owners/formulas approved; queries controlled and authorized at all stages; asynchronous work observable; exports protected; reconciliations and performance profile pass; reporting owner signoff/no critical defects.

## Constraints and Open Issues

Reporting primary DB/read model, KPI definitions, freshness, user concurrency, export limits, data retention, analytics access policy, query isolation, timezone/currency and external BI integration TBD/ADR Required.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Analytics and Reporting pack draft | Documentation Agent |
