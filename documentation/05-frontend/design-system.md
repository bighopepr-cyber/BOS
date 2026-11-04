# Design System Specification (DSS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-025 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Design System Owner |
| Related Documents | `ui-ux-specification.md`, `frontend-technical-specification.md`, `../../docs/MTS.md` |

## Tujuan dan Status

Mendefinisikan kategori token/komponen yang harus konsisten pada frontend BOS. MTS menetapkan Tailwind CSS dan component library yang konsisten, tetapi belum memilih library, brand, typography, warna, spacing scale, atau final token values. Dokumen ini tidak mengarang style guide visual.

## Token Architecture

Token semantic perlu dikategorikan untuk color roles (surface/text/border/status), typography roles, spacing, sizing, radius, elevation, focus, motion, layering dan density. Nilai, kontras, dark/light modes, branding, font assets, locale fallback, reduced-motion behavior TBD dan perlu design approval. Jangan hard-code warna berbeda di feature tanpa token.

## Component Catalog

MTS mengidentifikasi primitives, forms, data display, navigation, feedback, business components, module-specific components. Baseline inventory:

| Kategori | Komponen minimum |
|---|---|
| Primitive | Button, input, checkbox, radio, select, tooltip |
| Form | Date picker, validation/help text, upload, form field |
| Data | Table, pagination, sort/filter, badge, chart |
| Navigation | Sidebar, top navigation, breadcrumb, tabs, menu |
| Feedback | Alert, toast, modal/drawer, loading, empty/error state |
| Business | Approval/status timeline, currency/date/number display, file reference |

Component API, states (default/hover/focus/disabled/loading/error), keyboard behavior, responsive and accessibility requirements must be specified/tested before production use.

## Governance

One shared implementation/library decision; document versioning, contribution/accessibility review, deprecation and visual regression process. Module-specific patterns compose shared components and may not bypass authorization or validation.

## Open Issues

Component library/vendor, tokens/brand, type scale, density, dark mode, WCAG target, design review owner, icons/charts/datepicker dependencies, localization and visual test tooling.

## References / Change History

MTS §§4.2, 12.2, 12.5, 14; DOC-023/024.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Component catalog and unresolved visual decisions | Documentation Agent |
