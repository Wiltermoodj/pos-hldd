---
type: Reference
title: "apps/pos/src/lib/stripe-terminal/terminal.ts"
description: "Integrates with Stripe Terminal for physical in-store payments."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.852017Z }
tags: ['payments-&-ledger']
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
