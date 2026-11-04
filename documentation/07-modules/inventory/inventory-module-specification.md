# Inventory Management Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-INV |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Inventory/Finance Owner |
| Related Documents | `../../01-requirements/business-rules-specification.md`, `../../03-data/database-design.md`, `../../03-data/entity-relationship.md`, `../../02-architecture/integration-architecture.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Owns product/warehouse/location references and stock movement, transfer, adjustment, opname, balances and valuation. Stock movement is source history; costing policy remains open.

## 2. Functional Specification

Product/category/SKU/UOM; warehouse/location; stock balances; movement/transfer dispatch and receipt; adjustment/opname; batch/lot/serial when relevant; reorder levels and inventory valuation/reconciliation.

## 3. Business Process

Receive/issue/transfer/adjustment request → validate permission, product/location and policy → approval if configured → record immutable/auditable stock movement → update/rebuild consistent balance view → reconcile Finance valuation. Transfer requires dispatch then receipt.

## 4. Use Cases

Create product/warehouse; receive/issue stock through documented transaction; transfer between warehouses; record receipt; adjust/opname with audit; view stock and valuation. Process details depend on source transaction and policy.

## 5. Business Rules

All stock changes produce stock movement; never mutate balance without traceable transaction. Transfer has shipping and receiving process. Costing must be explicitly selected (e.g. FIFO/weighted average) before valuation. Reconcile inventory value to accounting. See RULE-INV.

## 6. Module Architecture

Inventory owns stock master/movement/balance/valuation. POS/Sales consume product/availability and request stock changes via contract; Finance consumes valuation/COGS mapping via approved contract. No module writes stock tables directly.

## 7. Database Design

MTS entities: products, product_categories, units_of_measure, warehouses, warehouse_locations, stock_movements, stock_movement_lines, stock_balances, stock_transfers, stock_adjustments, inventory_valuations. Optional lot/serial tables and costing layers TBD. PostgreSQL constraints/indexes per DOC-013.

## 8. Entity Relationship Diagram

See DOC-014: movement has lines; lines reference product/location; transfer/adjustment creates movements; balance summarizes product-location. Lot/serial and valuation relation TBD. Balance implementation must reconcile to movement history.

## 9. Data Dictionary

See DOC-015. Candidate SKU, UOM, warehouse/location, movement type/ref, quantity, effective time, source/destination, lot/serial and valuation cost. Quantity precision, UOM conversion, negative stock and cost fields TBD.

## 10. API Specification

Candidate `/api/v1/products`, `/warehouses`, `/stock-transfers`, `/stock-adjustments`, `/stock-movements`, `/stock-balances`. Endpoints must prevent direct balance mutation; require authorization, idempotency, state and audit per DOC-019.

## 11. Event & Queue Specification

Candidate movement posted, transfer dispatched/received, adjustment approved; POS/sales consumers and valuation reports may process asynchronously. Idempotent consumer and reconciliation required. Transactional outbox scope TBD in DOC-020.

## 12. UI/UX Specification

Product/warehouse directory; stock-by-location; transfer dispatch/receipt statuses; adjustment/opname with before/after evidence; movement history and valuation report. Clearly distinguish available/on-hand/reserved only after definitions approved.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| View stock/movement | `inventory.stock.view` | Assigned warehouse |
| Create transfer | `inventory.stock.transfer` | Source/destination scope |
| Dispatch/receive | `inventory.transfer.dispatch/receive` | Assigned location |
| Adjust/opname | `inventory.stock.adjust` | Restricted approval scope |
| Manage product/location | `inventory.product.manage`, `inventory.warehouse.manage` | Inventory admin |

Final assignments, threshold and separation of duties TBD.

## 14. Workflow Specification

Stock adjustment and transfer approval may be configured; MTS calls adjustment auditable and transfer dispatch/receipt. Approval thresholds, mismatch, cancellation/recall state TBD; DOC-022 engine applies when configured.

## 15. Integration Specification

POS/Sales request movement or availability; Finance reconciliation/valuation; Core organization scope/audit/workflow; Analytics read contract. Product master ownership with Sales/POS is TBD. Cross-domain contracts in DOC-011.

## 16. Validation Rules

Validate product/UOM/location, quantity precision/sign, source/destination difference, stock availability if policy, duplicate request, batch/serial requirements, transfer state, approval and movement reference. Negative stock/costing policies TBD.

## 17. Error Handling

422 invalid line/location/quantity; 409 insufficient stock/state/duplicate/concurrency; 403 warehouse scope; do not mutate balances partially; return actionable discrepancy status without hiding movement history.

## 18. Test Specification

Movement-only balance change; concurrent issue/reservation; transfer dispatch/receipt and partial discrepancy; adjustment audit/approval; idempotency; UOM/lot/serial; valuation/cost layers after decision; POS integration and Finance reconciliation.

## 19. Acceptance Criteria

All stock changes trace to movement; transfer lifecycle and adjustment audit pass; balances reconcile; costing selected and testable before valuation release; integration/permissions verified; owner signoff/no critical defect.

## Constraints and Open Issues

FIFO vs weighted average, negative stock, reservation, lot/serial scope, unit conversion, costing timing, COGS/Finance mapping, opening stock migration, discrepancy/cancellation, warehouse scope and reconciliation cadence TBD/ADR Required.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Inventory module pack draft, costing left open | Documentation Agent |
