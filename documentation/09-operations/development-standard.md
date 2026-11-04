# Code Convention & Development Standard

| Atribut | Nilai |
|---|---|
| Document Code | DOC-040 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Engineering Lead |
| Related Documents | `../04-backend/backend-technical-specification.md`, `../05-frontend/frontend-technical-specification.md`, `../08-testing/software-testing-specification.md`, `../02-architecture/module-architecture/module-architecture-specification.md`, `../../docs/MTS.md` |

## 1. Principles

Follow MTS: consistent naming, type safety, separation of concerns, dependency injection, small focused functions, no duplicated business logic/unnecessary abstractions, modular boundaries, tests, migration and updated docs. No direct cross-module database writes. Avoid business logic in controllers/UI.

## 2. Backend Standards

Laravel Form Request for request validation; Policy/authorization service for access; application service for complex use cases; transaction around atomic multi-table mutation; events/queue for asynchronous work; domain invariants in domain/application layer; repositories/adapters follow team-selected pattern. Use DB constraints where integrity requires. Posted finance history and stock movement must respect domain rules.

## 3. Frontend Standards

Strict TypeScript; feature-based organization; reusable shared component library; central API client and typed error handling; responsive/accessibility; separate server/local/form/auth/preferences state; frontend permission only for UX; no secrets in bundle; no blind retries for non-idempotent writes.

## 4. Git/Review

MTS branch examples: `main`, `develop`, `feature/*`, `bugfix/*`, `hotfix/*`. Change through PR/code review/automated tests; migration review; security review for sensitive changes. Branch enforcement, commit conventions, reviewers, CI tool and merge strategy TBD. Never commit environment secrets.

## 5. Definition of Done

Requirement satisfied and linked; UI/API/validation/permission present as needed; audit for critical actions; migration reviewed; unit/integration/API/E2E/security tests scoped and pass; error handling and docs updated; no unresolved critical defect; reviewer approval recorded. Release acceptance follows DOC-031.

## 6. Toolchain and Open Issues

Exact PHP/Laravel/Node/Next/PostgreSQL/Redis versions, formatter/linter/static analyzer, test framework, code coverage, CI, branch protection, dependency update cadence, API schema tooling and accessibility checks are TBD. Lock versions and publish compatibility matrix before implementation.

## References / Change History

MTS §§13–14, 17.4–17.5, 19; DOC-018/024/030/031/037.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Development conventions and DoD from MTS | Documentation Agent |
