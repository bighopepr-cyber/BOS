# Frontend Technical Specification (FTS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-024 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Frontend Architect |
| Related Documents | `information-architecture.md`, `ui-ux-specification.md`, `design-system.md`, `../04-backend/api/api-specification.md`, `../../docs/MTS.md` |

## Tujuan dan Baseline

Next.js + React + TypeScript frontend; Tailwind CSS; strict TypeScript; reusable components; feature-based organization; centralized API client; consistent error handling, responsive and accessible interactions. Next.js decision ADR-005 is Proposed; runtime versions TBD.

## Logical Structure

MTS conceptual layout: `src/app` for auth/dashboard/admin routes; `components` for shared primitives/forms/tables/layout; `features/<domain>` for domain components/hooks/services/types/schemas/routes/tests; `lib`, `hooks`, `services`, `types`, `styles`; public assets. Adapt routing conventions to selected Next.js version.

## Data and State

Separate server state, local UI state, form state, authentication state and global preferences. Avoid duplicating server cache across stores. Central API client sets base URL, credentials/CSRF policy per DOC-021, request ID, JSON encoding, typed result/error and safe retry policy. Mutations with financial/POS/inventory effects are not blindly retried without idempotency contract.

## Authorization and Security

Frontend may hide/disable actions based on permission-aware response but backend is authoritative. Never trust client permission/scope for data or mutation. Avoid secrets in bundle; protect XSS/CSRF/session boundary; use safe rendering and file links; error telemetry redacts user data. Auth storage/domain strategy TBD.

## Rendering, UX, and Resilience

Use layouts defined in DOC-026; forms/tables follow DOC-023/025. Provide loading/empty/error states and stale-data behavior. Long-running import/report flows use server job status. SSR/client boundaries, caching, offline support, realtime transport and PWA behavior are not prescribed by MTS and remain TBD.

## Testing/Build

Typecheck/lint/unit/component tests, API contract tests, accessibility checks, E2E key flows and production build are required by project CI strategy. Exact framework/testing package, browser matrix, coverage and bundle budgets TBD. Lock compatible versions at implementation start; review dependencies.

## Open Issues

Next.js/Node versions, API base URL/origin and CORS/cookie deployment, data-fetching/client cache library, form/schema library, component library, state tooling, SSR/cache policy, CSRF method, test tools and accessibility target.

## References / Change History

MTS §§4.2, 12, 14, 17.5, 18; DOC-023/025/026, DOC-019/021.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Frontend technical architecture and unresolved implementation choices | Documentation Agent |
