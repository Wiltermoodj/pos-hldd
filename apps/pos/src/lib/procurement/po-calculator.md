---
type: Reference
title: "apps/pos/src/lib/procurement/po-calculator.ts"
description: "Provides domain functionality for po-calculator."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.852440Z }
tags: ['procurement']
---

## Purpose & Responsibilities
- Provides domain functionality for po-calculator.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `calculatePOTotals(items: POItem[], shippingCostCents: number): POCalculationResult`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
