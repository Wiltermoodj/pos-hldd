---
type: Reference
title: "apps/pos/src/lib/workshop/repair-tag-printer.ts"
description: "Provides domain functionality for repair-tag-printer."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.851149Z }
tags: ['service-workshop']
---

## Purpose & Responsibilities
- Provides domain functionality for repair-tag-printer.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `executeReceiptPrint(workOrder: RepairWorkOrder): Promise<{ success: boolean, printError?: string }>`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
