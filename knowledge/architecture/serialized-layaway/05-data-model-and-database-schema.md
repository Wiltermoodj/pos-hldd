---
type: Concept
title: "Data Model and Database Schema"
---

# Data Model and Database Schema

## Production Drizzle ORM Schema
The database schema for the Serialized Inventory, Trade-ins & Layaways module is defined using Drizzle ORM and is typically located at `schema/serialized_layaway.ts`.

### `bike_assets`
This table tracks the physical bicycle and its associated components throughout its lifecycle.
- **Fields:**
  - `id`: Primary Key
  - Serial numbers: `frame_serial`, `battery_id`, `motor_serial`, `fork_id`
  - Asset details: `brand`, `model`, `year`, `color`, `size`
  - State tracking: `current_asset_state` (`IN_STOCK`, `SOLD`, `IN_SERVICE`, `TRADED_IN`, `ARCHIVED`)
  - Relations: `customer_id` (foreign key linking to the owner)

### `trade_in_records`
This table records the details of a trade-in transaction.
- **Fields:**
  - `id`: Primary Key
  - Relations: `customer_id`
  - Compliance data: `dl_number`, `dl_state`
  - Valuation data: `bbb_valuation_reference`, `trade_in_allowance`, `store_credit_vs_cash_flag`
  - Hold tracking: `police_hold_status`, `clear_date`

### `refurbishment_links`
This table bridges a trade-in record with the workshop work order required to prepare it for resale.
- **Fields:**
  - `id`: Primary Key
  - Relations: `trade_in_record_id`, `workshop_work_order_id`
  - Cost tracking: `refurb_parts_cost`, `refurb_labor_cost`, `calculated_final_cost_basis`

### `layaway_records`
This table manages the lifecycle and details of a customer layaway.
- **Fields:**
  - `id`: Primary Key
  - Relations: `customer_id`
  - Scheduling: `target_pickup_date`, `pdi_scheduled_status`
  - Location: `storage_rack_tag`
  - Financials: `deposit_amount_cents`, `remaining_balance_cents`
  - State tracking: `status` (`ACTIVE`, `PDI_SCHEDULED`, `COMPLETED`, `CANCELLED`)

### `layaway_payments`
This table records the individual payment installments made towards a layaway record.
- **Fields:**
  - `id`: Primary Key
  - Relations: `layaway_record_id`
  - Transaction details: `payment_amount`, `payment_method`, `stripe_terminal_charge_id`

## Compound Indexes & Relations
To ensure performance and maintain data integrity, the schema utilizes several optimized compound indexes and defined relations:

- **Serial Number Searches:** Indexes on `(brand, frame_serial)` to quickly locate specific bikes, especially during intake or service.
- **Customer History Lookups:** Indexes on `(customer_id, current_asset_state)` and `(customer_id, status)` (for layaways) to efficiently retrieve a customer's active bikes and ongoing transactions.
- **Active Layaway Statuses:** Indexes on `(status, target_pickup_date)` to optimize queries for the PDI scheduler and expiration alerts.
