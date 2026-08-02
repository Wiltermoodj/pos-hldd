---
type: Concept
title: "Data Model & Database Schema"
---

# Data Model & Database Schema

## Production Drizzle ORM Schema (`schema/pos_register.ts`)
The POS module utilizes Drizzle ORM configured for a Neon PostgreSQL database. The schema defines the core tables necessary for register operations and cash management.

- **`cash_shifts`**: Tracks the lifecycle of a register's cash drawer over a period.
  - Fields: Shift ID, store location ID, opened by user ID, closed by user ID, opening float, closing float, total cash sales, variance `amount_cents`, status (`OPEN`, `CLOSED`).

- **`cash_shift_transactions`**: Logs all movements of cash within a specific shift.
  - Fields: Shift ID, transaction type (`FLOAT_OPEN`, `CASH_SALE`, `CASH_DROP`, `PAID_OUT`), amount_cents, notes, user ID.

- **`register_sales`**: Records top-level transaction data.
  - Fields: Sale ID, receipt number, shift ID, customer ID, `subtotal_cents`, `discount_cents`, `tax_cents`, `total_cents`, payment status, sync status (`ONLINE`, `OFFLINE_PENDING_SYNC`).

- **`register_sale_items`**: Details the individual products or services purchased in a sale.
  - Fields: Sale ID, variant ID, quantity, `unit_price_cents`, `cost_cents`, `discount_amount_cents`, `line_total_cents`.

- **`register_sale_payments`**: Records the payment methods and amounts applied to a sale, supporting multi-tender splits.
  - Fields: Sale ID, payment method (`CASH`, `STRIPE_TERMINAL`, `STORE_CREDIT`, `GIFT_CARD`, `TRADE_IN_CREDIT`), amount_cents, Stripe payment intent ID reference.

## Compound Indexes & Constraints
To ensure data integrity and query performance, the database schema implements specific constraints and indexes:
- **Foreign Keys**: Enforces relational integrity linking sales to products, customers, and shifts.
- **Unique Constraints**: Unique constraints on receipt numbers to prevent duplicate transactions, especially crucial when merging offline-synced data.
- **Index Optimizations**: Compound indexes are strategically placed on frequently queried columns (e.g., store location + shift status, or customer ID + sale date) to ensure fast generation of shift reports and historical transaction lookups.
