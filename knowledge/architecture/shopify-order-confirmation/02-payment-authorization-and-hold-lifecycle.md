---
type: Concept
title: "Financial Engineering & Authorization Specifications"
---

# Financial Engineering & Authorization Specifications

## 1. Merchant Processing Fee Avoidance Math
Standard Auto-Capture E-Commerce Flow:
- Order Value: $1,000.00
- Processing Fee (2.9% + $0.30): $29.30 (Paid to payment processor immediately)
- If Item Sold Out & Refunded: Payment processor retains $29.30. Net Dealer Loss = -$29.30.

Authorization Hold Request Flow:
- Order Value: $1,000.00 (Card Authorization Hold Only)
- Processing Fee at Hold: $0.00
- If Item Sold Out & Voided: Card hold released. Net Dealer Loss = $0.00.

## 2. Canonical Drizzle Table Definition (`online_order_requests`)

Database schema definition for tracking online request lifecycles:

- Table Name: `online_order_requests`
- Primary Key: `id` (uuid, default random)
- Foreign Keys:
  - `location_id` (uuid, references `locations.id`)
  - `product_variant_id` (uuid, references `product_variants.id`)
- Columns:
  - `quantity` (integer, not null)
  - `payment_intent_id` (varchar 255, not null)
  - `authorization_amount_cents` (integer, not null)
  - `customer_email` (varchar 255, not null)
  - `customer_phone` (varchar 50)
  - `status` (enum: PENDING_STORE_CONFIRMATION, CONFIRMED_AND_CAPTURED, DECLINED_IN_STORE_PRIORITY, DECLINED_INVENTORY_DISCREPANCY, EXPIRED_VOIDED)
  - `created_at` (timestamp, default now)
  - `updated_at` (timestamp, default now)
