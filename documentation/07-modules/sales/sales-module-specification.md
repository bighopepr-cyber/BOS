# Sales Management Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-SALES |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Sales Business Owner |
| Related Documents | `../../01-requirements/frs.md`, `../../02-architecture/integration-architecture.md`, `../../03-data/database-design.md`, `../../04-backend/api/api-specification.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Owns quotation and sales order lifecycle; relates to CRM customers, products, discount/tax configuration and Finance invoice handoff.

## 2. Functional Specification

Quotation, items, discount, tax, validity, approval/PDF; sales order/items, confirmation, delivery status and invoice integration. MTS does not fully specify fulfillment, pricing or return behavior for non-POS sales.

## 3. Business Process

Select authorized customer/items → prepare quotation → validate configured price/discount/tax → approval if required → issue/accept quotation → create/confirm sales order → track delivery → request invoice via Finance contract. State transitions and handoff timing TBD.

## 4. Use Cases

Create/revise/approve quotation; convert accepted quotation to order; update delivery status; request invoice; view order history. Actor, approval and customer selection follow authorization scope. Detailed UC tests TBD.

## 5. Business Rules

Discount/tax are capability, not policy. No tax rate, currency, pricing strategy, quote validity duration, stock reservation, revenue recognition or approval threshold inferred. Cross-module writes prohibited.

## 6. Module Architecture

Sales owns quote/order and their snapshots/status. CRM provides customer reference; Inventory availability/fulfillment, Finance invoice creation via contract. Define immutable price/tax snapshots and corrections with Finance/Business owner before design.

## 7. Database Design

Candidate sales_quotations, quotation_items, sales_orders, sales_order_items, delivery status/history and source references. FK, identifiers, item snapshot, monetary types, uniqueness and delete policy TBD in DOC-013.

## 8. Entity Relationship Diagram

Conceptual: Customer reference 1—many quotations; quotation has items; accepted quotation may create order with items; order may reference invoice/delivery. Conversion cardinality, partial order and split invoice rules TBD.

## 9. Data Dictionary

See DOC-015; candidate fields customer reference, status, validity, item/product ref, quantity, unit price, discount, tax, currency, totals, delivery status, invoice reference. Precision, tax basis, snapshot rules TBD.

## 10. API Specification

Candidate `/api/v1/quotations`, `/quotations/{id}/approve`, `/sales-orders`, `/sales-orders/{id}/confirm`, `/sales-orders/{id}/invoice`. Final contracts require endpoint ID, auth/scope, request/response, validation/error, pagination/idempotency and audit per DOC-019.

## 11. Event & Queue Specification

Candidate quotation accepted/order confirmed/invoice requested; PDF generation and notifications may queue. Deduplicate conversion/invoice request; outbox/retry and status in DOC-020. Do not equate event delivery with invoice posting.

## 12. UI/UX Specification

Quotation editor/preview, approval state/history, order fulfillment status, customer/product lookup and export. Totals and discount/tax breakdown must be explainable after policy set; error/empty/loading states per DOC-023.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| View quotation/order | `sales.quotation.view`, `sales.order.view` | Assigned team/branch TBD |
| Create/update quotation | `sales.quotation.create/update` | Owner/team |
| Approve discount | `sales.quotation.approve` | Workflow/authority TBD |
| Confirm order | `sales.order.confirm` | Scope + state |
| Request invoice | `sales.order.invoice` | Authorized order |

Catalog and separation of duties TBD.

## 14. Workflow Specification

Quotation/discount approval is supported by MTS workflow examples; thresholds/routing and order approval are not defined. Use DOC-022; snapshot workflow version and history.

## 15. Integration Specification

CRM customer/contact; Inventory product/availability/fulfillment; Finance invoice; POS may share catalog but is separate transaction domain; Analytics reads controlled sales data. Ownership, sync and compensation in DOC-011 are open.

## 16. Validation Rules

Customer/item/reference scope, quantity, validity, allowed state transitions, configured discount/tax, duplicate conversion, idempotent invoice request. Numeric/rounding/tax and stock reservation policies TBD.

## 17. Error Handling

422 invalid item/policy; 409 stale quote/duplicate conversion/order state; 403 scope/approval; downstream Finance/Inventory failure recorded and recoverable; no silent invoice success. Standard API errors DOC-019.

## 18. Test Specification

Quote totals after policy; approval/scope; expiration; conversion idempotency; item snapshot; invoice handoff retry; delivery status; PDF permissions; concurrent updates; migration and reporting.

## 19. Acceptance Criteria

Approved pricing/tax/discount and fulfillment policies; quote-to-order traceability; invoice contract reconciles; permissions and workflow tested; audit/doc/API/test complete; Sales owner signs off.

## Constraints and Open Issues

Customer/product master ownership, price list, tax jurisdiction, discount authority, multi-currency, shipment, reservations, return/refund, invoice timing, PDF localization and numbering policy TBD.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Sales module pack draft | Documentation Agent |
