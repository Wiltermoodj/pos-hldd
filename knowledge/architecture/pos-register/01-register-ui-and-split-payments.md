---
type: Reference
title: "01 Register Ui And Split Payments"
description: "Knowledge document for 01 Register Ui And Split Payments."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.860213Z }
---

# POS Register UI & Split Payments

## Touchscreen Register Layout Specification
The POS application is optimized for touchscreen interactions, ensuring maximum efficiency during high-volume checkout periods.
- **Configurable 4x5 Quick-Keys Grid**: Allows instant adding of fast-moving items such as Tubes, Tires, Cables, Labor, and Water Bottles without searching.
- **Instant Search Bar**: Rapid product querying that returns results immediately as characters are typed.
- **Customer Lookup**: Fast search and selection of existing customers, or rapid creation of new customer profiles directly from the register.
- **Active Cart**: A highly visible, dynamic area that displays the current transaction lines, totals, and active discounts.

## Cart & Line-Item Operations
To provide flexibility at the checkout counter, the register supports the following line-item operations:
- **Quantity Modifiers**: Quickly adjust item quantities via on-screen plus/minus buttons or a numeric keypad overlay.
- **Price Overrides**: Manual override of the standard price, requiring an audit reason capture (e.g., "Price Match", "Damaged Item") to maintain accountability.
- **Line Discounts**: Apply percentage or flat-amount discounts to individual items in the cart.
- **Tax-Exempt Flags**: Toggle tax calculations off for specific line items or the entire transaction, useful for wholesale or non-profit customers.

## Multi-Tender Split Payment Engine
The checkout process features an intuitive multi-tender interface allowing a single sale to be split seamlessly across arbitrary combinations of tender types.
- **Arbitrary Tender Splits**: E.g., A $235.40 sale can be paid with $100 Cash, $50 Store Credit, and $85.40 on a Stripe Terminal Card.
- **Real-Time Remaining Balance**: As payments are applied, the system dynamically calculates and displays the remaining balance due.
- **Change Due Calculation**: For cash payments that exceed the remaining balance, the system automatically calculates and prominently displays the exact change due to the customer.
