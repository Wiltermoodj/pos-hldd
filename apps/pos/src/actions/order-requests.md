---
type: Reference
title: "apps/pos/src/actions/order-requests.ts"
description: "Provides domain functionality for order-requests."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.849617Z }
tags: ['ordering']
---

## Purpose & Responsibilities
- Provides domain functionality for order-requests.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `confirmOrderRequestAction(input: { requestId: string }): Promise<ActionResult<OnlineOrderRequest>>`
  - `declineOrderRequestAction(input: { requestId: string; reason: "DECLINED_IN_STORE_PRIORITY" | "DECLINED_INVENTORY_DISCREPANCY"; }): Promise<ActionResult<OnlineOrderRequest>>`
  - `createSpecialOrderAction(input: CreateSpecialOrderInput): Promise<ActionResult<{ orderId: string }>>`
  - `updateSpecialOrderTrackingAction(input: { orderId: string; trackingNumber: string }): Promise<ActionResult<{ success: boolean }>>`
  - `calculateVendorLeadTime(vendorId: string, orderDateStr: string): number`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
