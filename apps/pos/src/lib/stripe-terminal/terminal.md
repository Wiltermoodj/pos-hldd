---
type: Module
resource: apps/pos/src/lib/stripe-terminal/terminal.ts
domain: Payments & Ledger
status: Implemented
---


## Purpose & Responsibilities
- Integrates with Stripe Terminal for physical in-store payments.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `useStripeTerminal(): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Connection token generation.
- Payment intent capture and processing.

## Agreed-Upon Future Goals & Wishlist
- Better error handling for terminal disconnects.
- Support for tipping flow.
