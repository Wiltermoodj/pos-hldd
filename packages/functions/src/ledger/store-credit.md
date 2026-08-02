---
type: Reference
title: "packages/functions/src/ledger/store-credit.ts"
description: "Provides domain functionality for store-credit."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.846259Z }
tags: ['payments-&-ledger']
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
