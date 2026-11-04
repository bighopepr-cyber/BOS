# Core System Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-CORE |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Core Domain Owner |
| Related Documents | `../../02-architecture/module-architecture/module-architecture-specification.md`, `../../03-data/database-design.md`, `../../04-backend/authentication-authorization.md`, `../../04-backend/workflow-engine.md`, `../../01-requirements/frs.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Core provides organization hierarchy, identity, RBAC/data scope, workflow, notification, audit, document/file, numbering, custom fields, configuration and import/export. Shared capabilities do not own business-module transactions.

## 2. Functional Specification

Manage organization profile/hierarchy; users/sessions; roles/permissions; scope checks; workflow definitions/instances; notifications/delivery; audit/activity; file versions/relations; safe document sequences; typed custom fields; operational config; CSV/XLSX import/export and PDF export where applicable.

## 3. Business Process

User request → authenticate → permission/scope/state checks → execute domain use case → audit/transaction → async notification/job after commit. Organization/admin configuration requires privileged permission. Workflow generic process in DOC-005/022.

## 4. Use Cases

UC-CORE-001 login; UC-CORE-002 scoped resource authorization; UC-WF-001 submit/decide generic approval; admin manages user/role/config; file upload/download authorized. Preconditions and errors are in DOC-004/021.

## 5. Business Rules

One organization per installation; user != employee; backend is authorization authority; RBAC plus scope; do not log secrets; protected files outside public root; sequence concurrent-safe; workflow history protected; custom fields do not replace core relational fields. See DOC-006.

## 6. Module Architecture

Core subdomains: Organization, Identity, AccessControl, Workflow, Notification, Audit, Documents and SharedConfiguration. Expose contracts; no business module writes core-owned tables directly. Identity and Authorization should have explicit interface ownership.

## 7. Database Design

MTS initial entities: organizations, business_units, branches, divisions, departments, teams, positions, organization_hierarchies, users, user_profiles, user_sessions, password_reset_tokens, roles, permissions, role_permissions, user_roles, workflow_*, notifications_*, audit_logs/activity_logs, files/file_versions/document_categories/document_relations. PK, types, nullability, constraints/retention TBD in DOC-013.

## 8. Entity Relationship Diagram

See conceptual core ERD in DOC-014: Organization contains units; User-role-permission association; optional User-Employee link; workflow definition/version/step and instance/history; File versions. Exact cardinality/parent hierarchy TBD.

## 9. Data Dictionary

See DOC-015. Sensitive identity/session/audit/file metadata requires classification. Never persist password plaintext or secrets. Organization levels optional; role/permission and sequence uniqueness scope TBD.

## 10. API Specification

Global `/api/v1` envelope/auth/error/pagination in DOC-019. Candidate APIs: authentication session, organization hierarchy, user/role administration, workflow decision, authorized file access and import job. Endpoint list/body/permission must be finalized per contract; no CRUD auto-generation.

## 11. Event & Queue Specification

Candidates: workflow submitted/decided, notification delivery, import/export processing, file processing, audit event. Dispatch after commit/outbox strategy; idempotency/retry/timeout/status in DOC-020. Do not emit secrets or uncontrolled PII.

## 12. UI/UX Specification

Admin layout for users/roles/org/config; notification center; workflow inbox/history; file picker. Mask sensitive values, show only allowed actions; validation, empty/error/loading and keyboard states per DOC-023. Detailed IA/menu DOC-026.

## 13. Permission Matrix

| Capability | Candidate permission | Scope |
|---|---|---|
| View/update organization | `organization.profile.view/update` | Organization scope |
| Manage hierarchy | `organization.structure.view/create/update` | Assigned units |
| Manage users/roles | `identity.user.*`, `access.role.*` | Admin scope |
| Decide workflow | `workflow.instance.decide` | Assigned instance + domain record |
| Read audit | `audit.log.view` | Restricted, role TBD |
| Access file | `documents.file.view/download` | Parent record scope |
| Configure numbering/import | `system.configuration.update`, `system.import.create` | Admin/module scope |

Names and assignments are candidates, not approved catalog.

## 14. Workflow Specification

Generic definition/version/steps/assignment/conditional routing/delegation/revision/escalation/history per DOC-022. Per-process permission/state and version activation rules TBD.

## 15. Integration Specification

Shared contracts to every module: principal, permission/scope, workflow, audit, notification, file, numbering/config. Organization references may be consumed read-only; internal call/event/query contract in DOC-011. No cross-module write.

## 16. Validation Rules

Validate identity fields, organization parent/cycle constraints, active account, permission assignment, workflow version/state, file type/size/access, unique number under concurrency, custom-field type/value and import mapping. Exact formats/limits TBD.

## 17. Error Handling

401 unauthenticated, 403 forbidden, 404 disclosure policy, 409 duplicate/concurrency/state conflict, 422 validation, 429 throttling. Avoid revealing account/record secrets. Standard envelope DOC-019.

## 18. Test Specification

Auth success/failure/throttle; role/scope bypass and IDOR; organization hierarchy; concurrent numbering; workflow transition/history; file access/expiry; secret redaction; import duplicate/error; migration integrity. Test IDs in GOV-006, final IDs TBD.

## 19. Acceptance Criteria

Backend enforced auth/scope; shared APIs documented; hierarchy and workflows meet approved rules; history/audit protects secrets; sequence concurrency and files tested; module contract/integration tests pass; no critical defects; owner approval required.

## Constraints and Open Issues

RBAC policy, hierarchy schema, identifier strategy, authentication protocol, MFA, workflow conditions, custom-field persistence, file provider/limits, audit retention, import transaction policy and privileged owners TBD/ADR Required.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Core module pack draft covering 19 required topics | Documentation Agent |
