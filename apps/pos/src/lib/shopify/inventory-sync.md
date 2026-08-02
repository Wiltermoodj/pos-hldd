---
type: Module
resource: apps/pos/src/lib/shopify/inventory-sync.ts
domain: Inventory Ledger
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for inventory-sync.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `syncStockToShopify(variantId: string, newQuantity: number): Promise<boolean>`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
