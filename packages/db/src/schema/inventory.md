---
type: Reference
title: "packages/db/src/schema/inventory.ts"
description: "Handles inventory operations such as searching variants by barcode and checking out inventory sales."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.844741Z }
tags: ['inventory-ledger']
---

## Purpose & Responsibilities
- Handles inventory operations such as searching variants by barcode and checking out inventory sales.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - None explicitly exported / internal definitions
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Barcode lookups checking available stock considering reservations.
- Multi-location checkout processing with idempotency check.

## Agreed-Upon Future Goals & Wishlist
- Add stronger validation for checkout idempotency.
- Optimize concurrent reservations.
