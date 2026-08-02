---
type: Reference
title: "packages/functions/src/inventory/order-confirmation.ts"
description: "Provides domain functionality for order-confirmation."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.846524Z }
tags: ['inventory-ledger']
---

## Purpose & Responsibilities
- Provides domain functionality for order-confirmation.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `confirmOnlineOrderRequest(requestId: string): void`
  - `declineOnlineOrderRequest(requestId: string, reason: "DECLINED_IN_STORE_PRIORITY" | "DECLINED_INVENTORY_DISCREPANCY"): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
