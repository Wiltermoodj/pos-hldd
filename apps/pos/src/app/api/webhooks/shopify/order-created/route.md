---
type: Reference
title: "apps/pos/src/app/api/webhooks/shopify/order-created/route.ts"
description: "Provides domain functionality for route."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.848198Z }
tags: ['ordering']
---

## Purpose & Responsibilities
- Provides domain functionality for route.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `POST(req: Request): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
