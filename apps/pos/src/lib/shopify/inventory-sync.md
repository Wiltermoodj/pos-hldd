---
type: Reference
title: "apps/pos/src/lib/shopify/inventory-sync.ts"
description: "Provides domain functionality for inventory-sync."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.850168Z }
tags: ['inventory-ledger']
---

## Purpose & Responsibilities
- Provides domain functionality for inventory-sync.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `syncStockToShopify(variantId: string, newQuantity: number): Promise<boolean>`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
