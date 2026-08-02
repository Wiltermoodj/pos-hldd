---
type: Module
resource: packages/functions/src/ledger/store-credit.ts
domain: Payments & Ledger
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for store-credit.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `redeemStoreCredit(customerId: string, amountCents: number, referenceId: string): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
