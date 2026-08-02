---
type: Reference
title: "01 Online Order Request Flow"
description: "Knowledge document for 01 Online Order Request Flow."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.856082Z }
---

# Online Purchase Request & Authorization Lifecycle

## 1. Business & Architectural Purpose
Standard e-commerce platforms auto-capture payments immediately upon checkout. If an item is sold in-store simultaneously or has an inventory discrepancy, canceling the order forces the dealer to pay unrefundable credit card processing fees (2.9% + $0.30).

This system enforces an Authorization-First Request Workflow: online purchases create a credit card Authorization Hold and send a confirmation request to the store. Walk-in customers take priority, and unfulfilled online requests are voided with zero fee penalty.

## 2. System Architecture & Workflow Flowchart

[ Customer Checkout on Web ]
          │
          ▼
[ Payment Authorization Hold Created (No Funds Captured) ]
          │
          ▼
[ Create Order Request: Status = PENDING_STORE_CONFIRMATION ]
          │
          ▼
[ Increment `reserved_bopis` in Database ]
          │
          ▼
[ Push Real-Time Notification to In-Store POS Register ]
         / \
        /   \
  (Confirm) (Decline / In-Store Sale Overtake)
      /       \
     ▼         ▼
[ Capture Funds ]                [ Void Authorization Hold ($0 Fee) ]
[ Decrement `reserved_bopis` ]   [ Decrement `reserved_bopis` ]
[ Decrement `stocked_quantity` ] [ Restore `AvailableStock` ]
[ Send Confirmation SMS/Email ]  [ Send Out-of-Stock Notification ]

## 3. Transaction Execution Steps

### Step 1: E-Commerce Authorization Hold
When a customer checks out on the web storefront, the system issues a 7-day payment authorization hold via Stripe/Shopify Payments. Funds are reserved on the customer card, but no money is transferred, and zero merchant processing fees are assessed.

### Step 2: Request Insertion & Reservation
An incoming webhook creates an `online_order_requests` record set to `PENDING_STORE_CONFIRMATION`. An atomic database transaction increments `reserved_bopis` in `inventory_levels`, reducing `AvailableStock` for online shoppers while keeping `stocked_quantity` unchanged.

### Step 3: Register Notification & Staff Inspection
The physical POS register displays an overlay badge: "New Purchase Request". Register staff verify physical shelf availability or prioritize a walk-in customer currently purchasing the item at the counter.

### Step 4A: Store Order Confirmation
Staff tap "Confirm & Accept Order". The backend triggers payment capture (`paymentIntents.capture`). `reserved_bopis` and `stocked_quantity` both decrement by the order quantity, and the customer is notified that their order is ready for pickup or shipping.

### Step 4B: Store Order Decline (In-Store Overtake)
If a walk-in customer purchases the item first, staff tap "Decline - In-Store Priority". The backend cancels the payment authorization hold (`paymentIntents.cancel`). The card hold releases instantly, the merchant pays $0 in credit card processing fees, and `reserved_bopis` decrements to restore available stock.
