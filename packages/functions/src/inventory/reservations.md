---
type: Module
resource: packages/functions/src/inventory/reservations.ts
domain: Inventory Ledger
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for reservations.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `reserveInventory(variantId: string, locationId: string, quantity: number, type: "special_order" | "bopis" | "layaway" | "workshop"): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
