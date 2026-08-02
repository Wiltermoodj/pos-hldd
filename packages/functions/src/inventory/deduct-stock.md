---
type: Reference
title: "packages/functions/src/inventory/deduct-stock.ts"
description: "Provides domain functionality for deduct-stock."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.846789Z }
tags: ['inventory-ledger']
---

## Purpose & Responsibilities
- Provides domain functionality for deduct-stock.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `executeInventoryCheckout(input: ExecuteCheckoutInput, providedTx?: Tx): Promise<InventoryLevel[]>`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
