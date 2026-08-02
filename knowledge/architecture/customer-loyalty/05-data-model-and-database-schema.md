---
type: Reference
title: "05 Data Model And Database Schema"
description: "Knowledge document for 05 Data Model And Database Schema."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.854146Z }
---

# Data Model and Database Schema

## Production Drizzle ORM Schema (`schema/customer_loyalty.ts`)
The Customer Profiles, Family Accounts & Store Credit Module utilizes Drizzle ORM with Neon PostgreSQL. The schema is defined as follows:

### `customer_households`
- **Fields:**
  - `id`: Household ID (Primary Key)
  - `primary_customer_id`: ID of the primary account holder
  - `name`: Household name (e.g., "The Smith Family")
  - `created_at`: Created timestamp

### `household_members`
- **Fields:**
  - `household_id`: Reference to `customer_households.id`
  - `customer_id`: Reference to the individual customer profile
  - `role`: Relationship role (Enum: `PRIMARY`, `SPOUSE`, `CHILD`, `DEPENDENT`)

### `store_credit_ledger`
- **Fields:**
  - `id`: Transaction ID (Primary Key)
  - `customer_id`: Reference to the customer profile
  - `household_id`: Reference to the household (optional/nullable if strictly individual)
  - `amount`: Credit (+ positive) or Debit (- negative) amount
  - `balance_after`: Running balance after the transaction is applied
  - `transaction_type`: Type of transaction (e.g., `TRADE_IN_PAYOUT`, `RETURN_CREDIT`, `REGISTER_REDEMPTION`, `SHOPIFY_WEB_REDEMPTION`, `PROMOTIONAL_GRANT`)
  - `reference_id`: Source reference ID (e.g., Order ID, Trade-in ID)
  - `created_at`: Created timestamp

### `loyalty_accounts`
- **Fields:**
  - `customer_id`: Reference to the customer profile (Primary Key)
  - `current_balance`: Current available point balance
  - `lifetime_points`: Total points earned over the lifetime of the account
  - `tier_status`: Reward tier classification (Enum: `BRONZE`, `SILVER`, `GOLD`)

### `loyalty_ledger`
- **Fields:**
  - `id`: Transaction ID (Primary Key)
  - `customer_id`: Reference to the customer profile
  - `points`: Points added (+ positive) or redeemed (- negative)
  - `balance_after`: Point balance after the transaction
  - `line_item_reference`: Reference to the specific line item or order
  - `department_code`: Code of the department that generated the points

### `marketing_triggers`
- **Fields:**
  - `id`: Trigger ID (Primary Key)
  - `bike_asset_id`: Reference to the specific bike asset
  - `customer_id`: Reference to the customer profile
  - `trigger_type`: Type of marketing event (Enum: `KIDS_GROWTH_UPGRADE`, `CHAIN_SERVICE`, `ANNUAL_TUNEUP`)
  - `scheduled_dispatch_date`: Date the trigger is scheduled to fire
  - `status`: Current state of the trigger (Enum: `PENDING`, `SENT`, `CANCELLED`)

## Indexes & Foreign Keys
To ensure performance and data integrity, the schema implements the following constraints:
- **Unique Indexes:** Applied on `customer_id` within the `loyalty_accounts` table to enforce a 1:1 relationship between customers and loyalty accounts.
- **Compound Indexes:** Applied on `transaction_type` and `customer_id` within the `store_credit_ledger` to optimize queries calculating historical balances or filtering by event type.
- **Foreign Key Constraints:** Strict enforcement between `customer_id` and the core customer profiles table, and between `bike_asset_id` and the core bike asset inventory tables.
