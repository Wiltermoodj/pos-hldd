---
type: Module
resource: packages/db/src/schema/inventory.ts
domain: Inventory Ledger
status: Implemented
---


## Purpose & Responsibilities
- Handles inventory operations such as searching variants by barcode and checking out inventory sales.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - None explicitly exported / internal definitions
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Barcode lookups checking available stock considering reservations.
- Multi-location checkout processing with idempotency check.

## Agreed-Upon Future Goals & Wishlist
- Add stronger validation for checkout idempotency.
- Optimize concurrent reservations.
