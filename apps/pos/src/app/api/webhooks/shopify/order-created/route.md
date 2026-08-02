---
type: Module
resource: apps/pos/src/app/api/webhooks/shopify/order-created/route.ts
domain: Ordering
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for route.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `POST(req: Request): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
