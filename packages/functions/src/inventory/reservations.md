---
type: Reference
title: "packages/functions/src/inventory/reservations.ts"
description: "Provides domain functionality for reservations."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.846999Z }
tags: ['inventory-ledger']
---

## Purpose & Responsibilities
- Provides domain functionality for reservations.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `reserveInventory(variantId: string, locationId: string, quantity: number, type: "special_order" | "bopis" | "layaway" | "workshop"): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
