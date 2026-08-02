---
type: Reference
title: "04 Special Orders And Lifecycle Binding"
description: "Knowledge document for 04 Special Orders And Lifecycle Binding."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.862525Z }
---

# Special Orders and Lifecycle Binding

## Register Intake Workflow

The Special Order lifecycle begins at the touchscreen register UI:
1. **Item Selection:** A staff member adds an out-of-stock item (or a vendor-only item discovered via the JIT Gateway) to the cart.
2. **Customer Binding:** The POS strictly enforces the attachment of a Customer Profile to the ticket.
3. **Deposit Collection:** Using the Stripe Terminal SDK, the register prompts for a deposit based on store policy (either 50% or 100% of retail price).
4. **Link Creation:** Upon successful payment, a `special_order_links` record is instantiated.

## 7-Stage State Machine (`special_order_links.status`)

The lifecycle of a special order is rigorously tracked via a 7-stage state machine:

1. `QUEUED_FOR_PO`: The POS transaction is complete; the item is waiting in the reorder aggregator to be assigned to a vendor PO.
2. `PO_ATTACHED`: A manager has added the item to a Draft PO (`purchaseOrderItemId` is populated).
3. `ORDERED_FROM_VENDOR`: The PO has been successfully submitted to the distributor.
4. `RECEIVED_AT_STORE`: The GRN receiving process has logged the physical intake of the item.
5. `NOTIFIED_CUSTOMER`: Automated notifications have been dispatched to the customer.
6. `FULFILLED`: The customer has picked up the item, paid the remaining balance (if any), and the POS transaction is finalized.
7. `CANCELLED`: Terminal state if the order is aborted at any stage.

## Shelf-Protection & Reserve Guard

To prevent highly sought-after special order items from being accidentally sold to walk-in customers:
- **Scan Trigger:** During the GRN touchscreen receiving loop, when the barcode of a special order item is scanned, the UI flashes a high-priority, color-coded alert.
- **Hardware Trigger:** The POS automatically signals the receipt printer (via `@point-of-sale/receipt-printer-encoder`) to print a dedicated "Customer Reserve Tag" containing the customer's name, phone number, and order ID.
- **Logic Guard:** The unit's inventory status is immediately set to `RESERVED`, logically blocking it from standard register checkout flows unless the linked customer profile is attached to the sale.

## Omnichannel Shopify Sync

To maintain consistency across physical and digital channels:
- When a special order is taken in-store for an item synced with Shopify, an automatic `fulfillment_hold` is placed on the matrix variant via the Shopify GraphQL Admin API to prevent e-commerce overselling.
- Upon receiving the item in-store (`RECEIVED_AT_STORE` state), the backend triggers a `fulfillmentOrderReleaseHold` GraphQL mutation, safely releasing the hold for in-store fulfillment.

## Automated Notifications

Event-driven customer communication is triggered upon state transition to `RECEIVED_AT_STORE`:
- **SMS:** A concise text message is fired via the Twilio API (e.g., "Hi [Name], your special order [Item] has arrived at [Store Name]!").
- **Email:** A branded, detailed HTML email is sent via the Resend API, including store hours, pickup instructions, and any remaining balance due.

## Cancellation Protocol

Cancellations require different handling depending on the state of the order:
- **Pre-PO Submission (`QUEUED_FOR_PO`, `PO_ATTACHED`):** The deposit is refunded via Stripe, and the link is simply marked `CANCELLED`.
- **Post-Receiving (`RECEIVED_AT_STORE`):** The deposit refund is processed (subject to restocking fee policies), the link is `CANCELLED`, and the unit's status is reverted from `RESERVED` to `AVAILABLE_FOR_SALE`.
- **Vendor Backorders / Rejections:** If the distributor rejects the line item on the PO (e.g., out of stock), the system alerts a manager, detaches the PO link, reverts the state to `QUEUED_FOR_PO`, and prompts for alternative sourcing or customer cancellation.