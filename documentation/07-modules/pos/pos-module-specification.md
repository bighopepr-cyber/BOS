# Point of Sale Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-POS |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Retail/Finance Owner |
| Related Documents | `../../01-requirements/business-process-specification.md`, `../../02-architecture/integration-architecture.md`, `../../04-backend/api/api-specification.md`, `../../06-security-infrastructure/security-specification.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Owns in-person sales transaction, terminal/cashier shift, cart, payment status, receipt, returns/refunds and cash closing. Integrates Product/Inventory, Customer, Finance and Accounting.

## 2. Functional Specification

POS terminal/shift/cashier; product catalog; cart; discount/tax; payment methods; sales transaction/receipt; return/refund; cash closing and daily sales report. Offline mode is an explicit open decision.

## 3. Business Process

Open shift → build cart → validate price/tax/discount → submit checkout with idempotency → process payment → commit transaction → record Inventory movement/Finance handoff via contracts → issue receipt → close cash and reconcile. Exact cross-module atomicity and payment recovery TBD.

## 4. Use Cases

Open/close cashier shift; scan/select item; checkout/payment; reprint authorized receipt; return/refund; reconcile cash closing. Cashier, supervisor, finance operator and payment provider actors. UC-POS-001 describes candidate checkout.

## 5. Business Rules

POS transactions must be protected against duplicate requests, especially payment and stock recording. Shift, cart, payment, discount/tax, receipt, return/refund and closing are capability only; tax, tendering and return policy TBD.

## 6. Module Architecture

POS owns terminal/shift and retail transaction/payment state/receipt references. Inventory owns stock movements; Finance owns ledger/payment accounting; CRM may own customer. Integration is via idempotent contracts, no direct writes.

## 7. Database Design

Candidate pos_terminals, cashier_shifts, carts/cart_items, pos_transactions/lines, payments/payment_attempts, receipts, returns/refunds, cash_closings. Final entity types/provider tokens/retention and payment security design TBD; never store raw card credentials.

## 8. Entity Relationship Diagram

Conceptual: terminal has shifts; shift has cashier sessions and transactions; transaction has lines/payment attempts/receipt; return references original transaction; close aggregates shift cash movements. Cardinality and partial/void state TBD.

## 9. Data Dictionary

See DOC-015. Candidate transaction ID, terminal, cashier, shift, item/quantity/price snapshot, discount/tax, totals, payment method/status, idempotency reference, receipt and return reference. Money/currency/rounding and payment provider fields TBD.

## 10. API Specification

Candidate `/api/v1/pos/shifts`, `/pos/checkouts`, `/pos/transactions/{id}/returns`, `/pos/shifts/{id}/close`. Checkout requires idempotency contract, shift permission, validated payment state, duplicate/conflict errors and audit per DOC-019.

## 11. Event & Queue Specification

Candidate checkout committed, payment status changed, return accepted, shift closed; receipt/report generation may queue. Consumer idempotency, external payment webhook verification and reconciliation per DOC-020/011.

## 12. UI/UX Specification

Task-focused checkout/cart/payment/receipt, visible shift state, clear totals and payment pending/failure; cashier keyboard/device support TBD. Prevent accidental double-submit and show transaction reference. Return/refund uses separate verified flow.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| Open/use shift | `pos.shift.open`, `pos.checkout.create` | Assigned terminal/branch |
| Discount override | `pos.discount.override` | Threshold/supervisor TBD |
| Return/refund | `pos.return.create`, `pos.refund.issue` | Original transaction + approval |
| Close/reconcile | `pos.shift.close` | Assigned shift/supervisor |
| View reports | `pos.report.view` | Branch scope |

Final thresholds, role and payment access TBD.

## 14. Workflow Specification

Approval for discount/return/refund/cash variance may be configurable; MTS examples include sales discount approval, no threshold is given. Workflow definition/history through DOC-022; shift transition policy TBD.

## 15. Integration Specification

Product/customer lookup; Inventory stock movement; Finance payment/journal/reconciliation; Notification/receipt file; Analytics daily reports. Define source of truth, operation ID, idempotency, compensation and reconciliation before release.

## 16. Validation Rules

Active shift/terminal, product status/quantity, configured price/tax/discount, payment result, idempotency key, stock policy, original transaction eligibility for return, cash close totals. Exact tax, rounding and offline reconciliation TBD.

## 17. Error Handling

Payment timeout remains pending/unknown until reconciled; do not create duplicate payment or sale. 409 duplicate/stale shift, 422 invalid cart/payment, 403 terminal/branch scope. Provide safe recovery/status, not false success.

## 18. Test Specification

Duplicate checkout/retry, payment success/failure/timeout/webhook, shift isolation, stock movement consistency, Finance handoff/reconciliation, return/refund authorization, cash closing arithmetic after policy, receipt authorization, concurrency and backup restore.

## 19. Acceptance Criteria

Approved retail/tax/payment/refund rules; idempotent checkout; payment and stock/Finance reconciliation; audited shift/return; security and failure-recovery tests; POS owner approval/no critical defect.

## Constraints and Open Issues

Offline mode, provider/tokenization, tax jurisdiction, tender/currency/rounding, payment timeout, stock reservation, shift discrepancy, return/refund, receipt numbering, hardware/browser support and cross-module consistency ADR required.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | POS module pack draft, payment and offline policies TBD | Documentation Agent |
