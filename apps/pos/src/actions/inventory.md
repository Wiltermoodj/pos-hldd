---
# Module: apps/pos/src/actions/inventory.ts
**Domain Category:** Inventory Ledger
**Status:** Implemented

## Purpose & Responsibilities
- Handles inventory operations such as searching variants by barcode and checking out inventory sales.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `searchByBarcodeAction(input: { barcode: string, locationId: string }): Promise<ActionResult<(ProductVariant & { availableStock: number }) | null>>`
  - `checkoutSaleAction(input: CheckoutSaleInput): Promise<ActionResult<{ results: InventoryLevel[], orderId?: string }>>`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Barcode lookups checking available stock considering reservations.
- Multi-location checkout processing with idempotency check.

## Agreed-Upon Future Goals & Wishlist
- Add stronger validation for checkout idempotency.
- Optimize concurrent reservations.
---
