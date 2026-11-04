# Project Management Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-PROJECT |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Project Management Owner |
| Related Documents | `../../01-requirements/frs.md`, `../../02-architecture/module-architecture/module-architecture-specification.md`, `../../03-data/database-design.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Owns project, task, planning, timesheet and project finance tracking. It references HRM employee records and Finance financial data through contracts.

## 2. Functional Specification

Project profile/code/client/manager/members/budget/timeline/status; task/subtask/assignee/priority/due/dependency/checklist/attachment/comment; milestones/Gantt data/WBS/resource allocation; timesheets/worklogs/billable hours/approval/productivity report; budget/cost/expense allocation/revenue/profitability.

## 3. Business Process

Create project and assign authorized members → define milestones/tasks/dependencies → assign work → log time/expense → approve entries if configured → aggregate costs/revenue against budget → report status. Finance transaction is not duplicated as project ledger.

## 4. Use Cases

Create project; plan milestone/task; assign member; update task/dependency; submit/approve timesheet; allocate project expense; view budget/cost/profitability. Client actor/portal not specified by MTS.

## 5. Business Rules

MTS defines capability only. Project code uniqueness, WBS/dependency cycle, billable policy, time rounding, capacity, budget thresholds and profitability calculation need owner decisions.

## 6. Module Architecture

Project owns project/task/milestone/timesheet/resource and project analytical references. HRM owns employee; Finance owns ledger/payments; use read/command contracts. Collaboration may link comments/attachments, not own project state.

## 7. Database Design

Candidate projects, project_members, tasks, task_dependencies, milestones, resource_allocations, timesheets, work_logs, project_budgets/cost references. Exact entity fields and whether finance amounts are copied or referenced TBD.

## 8. Entity Relationship Diagram

Conceptual: Project has members/tasks/milestones/timesheets; tasks may parent/subtask and depend on tasks; time entries reference project/task and employee; project cost lines reference Finance expense/source. Cardinality and cycle constraints TBD.

## 9. Data Dictionary

See DOC-015; candidate project code/client/status/date/budget, task priority/due/assignee/dependency, time duration/billable flag, financial references. Currency, duration precision and status dictionary TBD.

## 10. API Specification

Candidate `/api/v1/projects`, `/projects/{id}/tasks`, `/tasks/{id}/dependencies`, `/timesheets`, `/timesheets/{id}/submit`, reports. Final endpoint authorization, validation, pagination/idempotency in DOC-019.

## 11. Event & Queue Specification

Candidate project/task assigned, milestone changed, timesheet submitted/approved; notifications, scheduled reports and exports asynchronous. Ensure duplicate time submission does not double bill; queue contracts DOC-020.

## 12. UI/UX Specification

Project overview, task board/list/timeline, milestone/Gantt view, timesheet entry, approval queue, cost/budget comparison. MTS names Gantt data but does not choose visualization/library; permissions govern project scope.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| View project/task | `project.project.view`, `project.task.view` | Member/assigned scope |
| Manage plan/task | `project.task.create/update/assign` | Project manager/authorized scope |
| Submit timesheet | `project.timesheet.create/submit` | Own record |
| Approve time/budget | `project.timesheet.approve`, `project.budget.approve` | Assigned workflow |
| View cost | `project.finance.view` | Restricted project scope |

Final policy and client access TBD.

## 14. Workflow Specification

Timesheet approval is supported by MTS; project budget approval also named. Assignment, threshold, revision and escalation remain configuration/business decisions per DOC-022.

## 15. Integration Specification

HRM employee/member reference; Finance budget/cost/expense/revenue; Collaboration comments/activity; Analytics project reports. No direct writes into HRM/Finance. Project/client identity ownership TBD.

## 16. Validation Rules

Project member/scope, date consistency, task dependency valid/no cycle if policy, valid employee/task, timesheet duration/date, approval state, budget/expense references. Formula and boundaries TBD.

## 17. Error Handling

422 invalid task/time/dependency; 409 stale task/duplicate timesheet/cycle; 403 project scope; finance integration errors visible as pending/error rather than false posted state.

## 18. Test Specification

Task dependency cycle and project scope; timesheet duplicate/approval; time totals and billability; budget/expense references; HRM/Finance contract; report access; migration and async retries.

## 19. Acceptance Criteria

Project/task planning, permission and time workflow satisfy approved rules; financial tracking reconciles to Finance; dependencies/integrations tested; reports and docs complete; owner acceptance/no critical defect.

## Constraints and Open Issues

Project code format, client master, Gantt, time rounding/billable rules, budget approval, cost allocation, revenue/profit formula, calendar/holidays and client portal TBD.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Project Management module pack draft | Documentation Agent |
