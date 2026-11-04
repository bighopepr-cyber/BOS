# HRM / HRIS Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-HRM |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — HR Business Owner |
| Related Documents | `../../01-requirements/frs.md`, `../../02-architecture/module-architecture/module-architecture-specification.md`, `../../03-data/database-design.md`, `../../04-backend/workflow-engine.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Owns employee lifecycle and HR records; employee is distinct from authentication User. Subdomains: employee, recruitment, attendance, leave, payroll, performance, training.

## 2. Functional Specification

Employee profile/number, personal/employment info, position/unit/branch/manager/status, contract history, documents/emergency contact; vacancy/application/candidate/interview/evaluation/hiring/onboarding; attendance/clock/shift/schedule/overtime/correction; leave types/balance/request/approval/calendar/history; configurable payroll components/period/calculation/approval/payslip; KPI/goals/review/competency; training assignments/records/certifications.

## 3. Business Process

Employee lifecycle: authorized HR creates/updates employee → assign organization/employment state → attach protected documents → audit lifecycle. Leave: employee submits → validate balance/policy → workflow approval → update leave record/balance/history → notify. Process details depend on HR policy and DOC-022.

## 4. Use Cases

Create employee; change employment status; submit/approve leave; capture/correct attendance; calculate and approve payroll; assign training and record completion. Actors: HR operator, employee, manager/approver, payroll operator. Detailed scenarios TBD; pattern UC-WF-001.

## 5. Business Rules

MTS requires employee lifecycle, attendance, leave, payroll, performance, recruitment and training capabilities. Payroll must follow configured jurisdiction-specific policy; no formula, entitlement, overtime, leave accrual or legal rule is inferred.

## 6. Module Architecture

HRM owns Employee and HR subdomain records. Core Identity may link User optionally; Organization provides structure reference; Finance receives payroll journal/result only via approved contract. Protect HR data and expose minimal references.

## 7. Database Design

Candidate entity groups: employees, employment/contract history, emergency contacts, vacancies/applications/candidates/interviews/evaluations, attendance/schedules/shifts/overtime, leave types/balances/requests, payroll periods/components/results/payslips, performance goals/reviews, training assignments/records/certifications. Entity names/schema not all specified in MTS; DDS review required.

## 8. Entity Relationship Diagram

Conceptual: Employee relates to Organization position/department/branch and optional User; Employee has contract history, attendance, leave requests, training/performance records; payroll run groups employee results by period. Cardinality and relation ownership TBD; no final ERD until rules approved.

## 9. Data Dictionary

See DOC-015; employee number, employment status, dates, currency/payroll amounts and classification need owner definition. Personal, payroll and health/emergency data require classification and restricted access; retention TBD.

## 10. API Specification

Candidate resources `/api/v1/employees`, `/leave-requests`, `/attendance`, `/payroll-periods`, `/training-records`; operations must have explicit IDs, request/response, validation, permission, scope, errors and pagination in API catalog. Payroll amount/formulas not specified. No endpoints final in this pack.

## 11. Event & Queue Specification

Candidate leave submitted/approved, employee lifecycle changed, payroll approved, training completed; queue email/notification/report/export. Event payload minimum and external payroll integration TBD; use DOC-020 retry/idempotency and avoid sensitive payload.

## 12. UI/UX Specification

Employee directory/profile, leave request/inbox, attendance views, recruitment pipeline, payroll restricted workspace, performance/training views. Field visibility based on backend permissions; sensitive payroll/personal data masked. Screen/menu follows DOC-023/026.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| View employee | `hrm.employee.view` | Own/department/branch/assigned |
| Create/update employee | `hrm.employee.create/update` | HR-authorized scope |
| Request leave | `hrm.leave.create` | Own record |
| Approve leave | `hrm.leave.approve` | Assigned workflow + scope |
| View payroll | `hrm.payroll.view` | Restricted payroll scope |
| Calculate/post payroll | `hrm.payroll.calculate/approve` | Authorized payroll role |

Final catalog, field-level permissions and segregation of duties TBD.

## 14. Workflow Specification

Leave approval is explicit MTS use; attendance corrections, recruitment decision, payroll approval may use workflow if configured. Approver matrix, delegations, thresholds and states TBD; use DOC-022 generic engine.

## 15. Integration Specification

Core Identity optional link; Organization structure/scope; Workflow/Notification/Documents/Audit shared; Finance payroll journal handoff; LMS/Analytics employee/training read contracts. No direct writes into Finance or Core tables. Payroll integration scope TBD.

## 16. Validation Rules

Validate employee number uniqueness scope TBD, dates/status transitions, organization references, leave balance/policy, attendance time/shift, payroll period/input components, candidate/application state and required document permission. Exact policies and rounding TBD.

## 17. Error Handling

422 invalid HR data; 403 scope/field access; 404 or disclosure-safe response; 409 stale employment/workflow/payroll state; asynchronous job errors surfaced by status. Do not expose confidential HR data.

## 18. Test Specification

Employee/User distinction; scope and field confidentiality; lifecycle transitions; leave balance concurrency/approval; attendance correction audit; payroll arithmetic/rounding only after policy approved; workflow duplication; export/download security; migration and HR-to-Finance contract tests.

## 19. Acceptance Criteria

Approved HR policies and fields; lifecycle/use cases and permissions documented; sensitive data protected; configured payroll results reconcile; leave/attendance/workflow histories tested; integration contracts and docs complete; no critical defects; HR owner signs off.

## Constraints and Open Issues

Jurisdiction, payroll full module vs integration, formulas/tax, employment policies, leave accrual, attendance device, data classification/retention, employee ID, finance mapping and HR approvers TBD.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | HRM pack draft; policies intentionally left open | Documentation Agent |
