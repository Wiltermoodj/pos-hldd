---
# Module: packages/functions/src/inventory/order-confirmation.ts
**Domain Category:** Inventory Ledger
**Status:** Implemented

## Purpose & Responsibilities
- Provides domain functionality for order-confirmation.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `confirmOnlineOrderRequest(requestId: string): void`
  - `declineOnlineOrderRequest(requestId: string, reason: "DECLINED_IN_STORE_PRIORITY" | "DECLINED_INVENTORY_DISCREPANCY"): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
---
