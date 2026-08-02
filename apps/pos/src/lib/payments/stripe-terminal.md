---
# Module: apps/pos/src/lib/payments/stripe-terminal.ts
**Domain Category:** Payments & Ledger
**Status:** Implemented

## Purpose & Responsibilities
- Integrates with Stripe Terminal for physical in-store payments.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `createTerminalConnectionToken(): Promise<string>`
  - `captureTerminalPayment(paymentIntentId: string): Promise<Record<string, unknown>>`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Connection token generation.
- Payment intent capture and processing.

## Agreed-Upon Future Goals & Wishlist
- Better error handling for terminal disconnects.
- Support for tipping flow.
---
