---
type: Reference
title: "packages/functions/src/inventory/order-requests.ts"
description: "Provides domain functionality for order-requests."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.847186Z }
tags: ['inventory-ledger']
---

## Purpose & Responsibilities
- Provides domain functionality for order-requests.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `createOnlineOrderRequest(input: CreateOrderRequestInput): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
