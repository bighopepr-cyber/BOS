# Finance & Accounting Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-FIN |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Finance Controller |
| Related Documents | `../../01-requirements/business-rules-specification.md`, `../../03-data/database-design.md`, `../../04-backend/api/api-specification.md`, `../../04-backend/workflow-engine.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Owns chart of accounts, fiscal/accounting periods, journals/ledger, AR/AP, cash/bank, expense, budget and financial reports. Finance and Accounting stays distinct from Sales/POS source transactions.

## 2. Functional Specification

CoA, journal entry/line, validation/post/reversal/adjustment, GL/trial balance/period closing; invoices/payment/credit notes/aging; bills/vendor credits/payables; bank/cash transactions/reconciliation; expense claims/receipts/approval/reimbursement; budgets/variance; balance sheet/income/cash flow and other reports.

## 3. Business Process

Source business transaction → Finance contract creates draft/source reference → validate accounts/period/amounts → optional approval → post balanced journal atomically → ledger/report update → audit. Corrections use reversal/adjustment, not direct edit. Detailed BPS-FIN-001 in DOC-005.

## 4. Use Cases

Configure account/period; create/validate journal; approve/post journal; reverse/adjust; record receivable/payable/payment; reconcile bank; approve expense; report trial balance/aging. Roles are conceptual; exact actor rights TBD.

## 5. Business Rules

Every posted journal balances; posted journals immutable via normal flow; correction through reversal/adjustment; periods can lock; transactions have source reference/audit; money uses precise numeric not float. See RULE-FIN-001–004. Accounting policy beyond these facts remains TBD.

## 6. Module Architecture

Finance owns ledger, period/account, invoices/bills/payment records, budget and finance reporting. Sales/POS/HRM/Project send source data via public contracts; Finance determines journal mapping only after business approval. Avoid cross-module DB writes.

## 7. Database Design

MTS entities: chart_of_accounts, accounting_periods, fiscal_years, journal_entries, journal_lines, bank_accounts, payments, invoices, bills. Candidate AP/AR/budget/expense entities require detailed design. Numeric precision, currency, dimensions, FK action, posting lock/unique constraints and soft-delete policy TBD.

## 8. Entity Relationship Diagram

See DOC-014: accounting period has journal entries; entry has one-or-more lines; lines reference CoA; entries reference source and may be corrected by reversal/adjustment. Exact cardinality, currency and reversal links TBD.

## 9. Data Dictionary

See DOC-015. Journal line minimum: journal_entry_id, account_id, debit, credit, currency_code if multicurrency, description, dimensions/reference if needed. Amount type is exact numeric; precision/scale and currency_code requirement await decision.

## 10. API Specification

Candidate `/api/v1/journal-entries`, `/journal-entries/{id}/post`, `/accounts`, `/accounting-periods`, `/invoices`, `/payments`, `/reports/trial-balance`. DOC-019 defines endpoint requirements; examples are not final route/payload contract. Sensitive finance endpoints require permission/scope and period-state check.

## 11. Event & Queue Specification

Candidate `JournalPosted`, `InvoiceIssued`, `PaymentRecorded`, `PeriodClosed`; report generation async. Consumers idempotent and source-correlated. Do not publish unapproved account/person/payment detail. Event schema/outbox/ordering in DOC-020 TBD.

## 12. UI/UX Specification

Journal editor with balanced totals/status, account/period validation, source references and history; restricted payment/bank workspaces; aging/budget reports with filters. Posted entries show immutable state and permitted correction route. Number/currency formatting follows configured locale; locale policy TBD.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| View journal/report | `finance.journal.view`, `finance.report.view` | Company/unit/account scope TBD |
| Create/update draft | `finance.journal.create/update` | Authorized period |
| Post/reverse | `finance.journal.post/reverse` | Role + open period + workflow |
| Manage CoA/period | `finance.account.manage`, `finance.period.manage` | Finance admin |
| Pay/reconcile | `finance.payment.create`, `finance.bank.reconcile` | Restricted account scope |

Final role, SoD, approval limits and company scope TBD.

## 14. Workflow Specification

MTS supports journal/expense/budget approvals. Posting may require configured workflow; period closing/adjustment authority and reopen rules need owner approval. Approval history per DOC-022.

## 15. Integration Specification

Sales invoice handoff; POS sales/payment; HRM payroll result; Project cost/revenue/expense; Inventory valuation/COGS. Finance is system of record for accounting entries, but source contract/mapping/timing/reconciliation are TBD. No table writes by source modules.

## 16. Validation Rules

Balanced debit/credit; valid account and source; open period; exact numeric amount; valid workflow/permission; duplicate/idempotency; posted immutability. Multi-currency rate, zero line, rounding, closed-period adjustment and invoice tax rules TBD.

## 17. Error Handling

422 unbalanced/invalid account; 409 closed period/stale posting/duplicate; 403 permission/scope; no partial post on failure. Error response must not expose sensitive financial data. Use reversal/adjustment workflow.

## 18. Test Specification

Balanced/unbalanced journal; concurrency/idempotent post; period lock; immutable posted entry; reversal reference; decimal precision once approved; source integration reconciliation; report totals and access; audit and migration; backup restore reconciliation.

## 19. Acceptance Criteria

Approved accounting policies; journals balance and immutable; period/permission rules enforced; source-to-ledger traceability; audit and reports reconcile; contracts documented; Finance owner acceptance and no critical defects.

## Constraints and Open Issues

Multi-company, chart/accounting standards, fiscal periods/close, multicurrency, tax, precision/rounding, payroll mapping, costing/valuation, approval thresholds, retention and legal jurisdiction all TBD/ADR Required.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Finance pack aligned to explicit MTS accounting invariants | Documentation Agent |
