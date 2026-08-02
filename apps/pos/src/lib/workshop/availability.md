---
type: Reference
title: "apps/pos/src/lib/workshop/availability.ts"
description: "Provides domain functionality for availability."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.851339Z }
tags: ['service-workshop']
---

## Purpose & Responsibilities
- Provides domain functionality for availability.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `getAvailableRepairSlots(ctx: AvailabilityContext, existingWorkOrders: MockWorkOrder[]): string[]`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
