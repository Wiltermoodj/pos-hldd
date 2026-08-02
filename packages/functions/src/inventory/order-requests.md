---
type: Module
resource: packages/functions/src/inventory/order-requests.ts
domain: Inventory Ledger
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for order-requests.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `createOnlineOrderRequest(input: CreateOrderRequestInput): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
