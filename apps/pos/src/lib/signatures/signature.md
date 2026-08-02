---
type: Reference
title: "apps/pos/src/lib/signatures/signature.ts"
description: "Provides domain functionality for signature."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.851802Z }
tags: ['pos-register']
---

## Purpose & Responsibilities
- Provides domain functionality for signature.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `processSignatureBase64(base64String: string): Buffer`
  - `linkSignatureToWorkOrder(base64String: string, workOrder: WorkOrderType): WorkOrderType`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
