---
type: Module
resource: apps/pos/src/lib/procurement/po-calculator.ts
domain: Procurement
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for po-calculator.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `calculatePOTotals(items: POItem[], shippingCostCents: number): POCalculationResult`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
