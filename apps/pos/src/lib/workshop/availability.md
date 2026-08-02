---
type: Module
resource: apps/pos/src/lib/workshop/availability.ts
domain: Service Workshop
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for availability.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `getAvailableRepairSlots(ctx: AvailabilityContext, existingWorkOrders: MockWorkOrder[]): string[]`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
