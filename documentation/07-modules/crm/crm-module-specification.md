# CRM Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-CRM |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — CRM Product Owner |
| Related Documents | `../../01-requirements/frs.md`, `../../02-architecture/module-architecture/module-architecture-specification.md`, `../../02-architecture/integration-architecture.md`, `../../04-backend/api/api-specification.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Owns lead and customer relationship data, contacts, pipelines/opportunities, interaction history and follow-ups. Customer legal/accounting master ownership with Sales/POS is an open contract question.

## 2. Functional Specification

Lead capture/source/assignment/scoring/qualification/conversion; contact/customer/organization profile and relationship mapping; pipeline/stage/opportunity/probability/expected close/forecast; call, meeting, email log, notes, follow-up and activity history.

## 3. Business Process

Capture lead → validate/deduplicate → assign → qualify → progress opportunity through configured stages → convert to customer/reference for Sales → record interactions/follow-ups. Scoring, conversion criteria and lifecycle state are TBD; no automation is presumed.

## 4. Use Cases

Create/assign lead; qualify/convert lead; update contact/customer relationship; move opportunity stage; log meeting/call/follow-up; view authorized pipeline/forecast. Actors: sales staff/manager and authorized integration. Detailed state transitions TBD.

## 5. Business Rules

MTS lists lead source/assignment/scoring/qualification/conversion, pipeline/stage/probability and interaction history; it does not define scoring formula, mandatory fields, duplicate match or customer lifecycle policy. Do not infer them.

## 6. Module Architecture

CRM owns lead and relationship history. Expose customer/contact read or conversion contract to Sales/POS; avoid creating conflicting master writes. Organization, Identity, scope, audit and notification are shared core.

## 7. Database Design

Candidate entities: leads, lead_sources, contacts, customers/organizations, relationship mappings, pipelines, stages, opportunities, activities/interactions/followups. Exact table list/ownership (especially customer vs Sales/POS), constraints and identity matching TBD; PostgreSQL rules in DOC-013.

## 8. Entity Relationship Diagram

Conceptual: Lead may convert/link to customer/contact; opportunity belongs to pipeline/stage and references customer/contact; activities link to lead/contact/opportunity. Conversion cardinality/history and deduplication semantics TBD; no physical schema implied.

## 9. Data Dictionary

See DOC-015. Candidate attributes source, status, owner, score, expected close date, probability, activity type, contact method; definitions, PII classification, uniqueness and retention TBD.

## 10. API Specification

Candidate collection resources `/api/v1/leads`, `/contacts`, `/opportunities`, `/activities`; conversion and stage transition actions require explicit contract. Every route needs permission/scope, validation, filter/pagination and error contract per DOC-019; paths illustrative only.

## 11. Event & Queue Specification

Possible events lead qualified/converted, opportunity stage changed, follow-up due; notifications and exports can queue. Avoid duplicate conversion and unauthorized payload delivery; event semantics/schema/retry in DOC-020 TBD.

## 12. UI/UX Specification

Lead list/detail, pipeline view, contact history and follow-up views; show ownership/stage and allowed actions; preserve filters/pagination; prevent unauthorized export. Forecast definitions and screen layout user research TBD.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| View/export customer | `crm.customer.view/export` | Assigned team/organization scope TBD |
| Manage lead | `crm.lead.create/update/assign/convert` | Owner/team scope |
| Manage opportunity | `crm.opportunity.view/update` | Pipeline/team scope |
| Record activity | `crm.activity.create` | Related authorized record |
| Configure pipeline | `crm.pipeline.manage` | CRM admin |

Final sharing model and export permissions TBD.

## 14. Workflow Specification

Lead assignment, qualification or conversion approval may be configured if business requires; MTS does not mandate approval. Any workflow uses DOC-022 and an approved CRM policy.

## 15. Integration Specification

Sales consumes customer/contact and opportunity context; Collaboration may provide meeting/communication references; Analytics consumes controlled pipeline data; Core provides user/org/access/audit/notification. Integration direction and master data owner must be resolved.

## 16. Validation Rules

Validate required source/status/owner according to approved setup; stage transition membership; date/probability range and duplicate checks only after policies set; permission/scope on related records; safe text/email/URL validation.

## 17. Error Handling

422 invalid stage/fields; 409 duplicate/concurrent conversion or stale stage; 403 scope; 404 disclosure-safe; external email integration failures do not falsify CRM state. Errors follow DOC-019.

## 18. Test Specification

Lead lifecycle and duplicate handling; conversion idempotency and relationship preservation; stage validation; team/record scope and exports; activity history/audit; filtering/pagination; integration contracts; migration and report authorization.

## 19. Acceptance Criteria

Approved customer ownership and CRM policies; lead/opportunity workflows and permissions verified; conversion preserves references without duplicate master; audit and integration tests pass; CRM owner signoff/no critical defect.

## Constraints and Open Issues

Customer system-of-record, field/PII classification, dedupe, score/forecast definitions, pipeline stages, integration to Sales/POS, communication provider, retention, visibility/sharing and approval policies TBD.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | CRM pack covering 19 required topics with policy TBDs | Documentation Agent |
