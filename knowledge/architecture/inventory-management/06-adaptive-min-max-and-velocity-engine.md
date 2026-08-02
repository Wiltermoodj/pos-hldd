---
type: Concept
title: "Adaptive Min/Max & Sales Velocity Engine Specifications"
---

# Adaptive Min/Max & Sales Velocity Engine Specifications

## 1. Domain Objective
Standard static min/max thresholds lead to capital lockup in slow-moving inventory and stockouts on high-turnover consumables. This document defines the architectural blueprint for an automated algorithm that computes dynamic min/max stock suggestions based on true sales velocity, vendor order frequencies, discount sensitivity, and stockout opportunity loss.

## 2. Core Metric Inputs

### A. In-Stock Normalized Velocity
Sales velocity must only evaluate periods when physical stock was present to prevent zero-inventory days from artificially dampening true customer demand.
- Calculation: `inStockDailyVelocity` = total_units_sold_in_window / count_of_days_stocked_greater_than_zero
- Impact: Prevents popular items that were out of stock for 25 out of 30 days from showing artificially low daily demand.

### B. Vendor Cadence & Lead Time
- Fields: `vendor_lead_time_days` (time from PO creation to receiving) and `vendor_order_cycle_days` (frequency of routine PO placements with supplier).
- Lead time dictates the baseline safety buffer required to prevent stockout prior to delivery.

### C. Margin & Discount Sensitivity Adjustment
- Rule: Sales velocity spikes driven by promotional or clearance pricing MUST NOT inflate full-price reorder targets.
- Threshold: Any transaction line item where `discount_amount_cents` > 0 or sold below baseline `retail_price_cents` applies a discount-weighting multiplier (e.g., `discountMultiplier` = 0.5) to normalize demand toward organic, full-margin velocity.

### D. Stockout Opportunity Loss Analysis
- Tracks `days_out_of_stock` while product was active.
- Computes estimated lost unit sales to adjust baseline safety thresholds dynamically upon replenishment.

## 3. Mathematical Models & Formulae

1. Base Safety Buffer (Suggested Min):
   `suggestedMin` = (`inStockDailyVelocity` * `vendor_lead_time_days`) + `safetyVarianceBuffer`

2. Stock Ceiling (Suggested Max):
   `suggestedMax` = `suggestedMin` + (`inStockDailyVelocity` * `vendor_order_cycle_days`)

## 4. Execution Strategy & Architectural Placement

- Batch Processing Job: Calculated via an asynchronous scheduled job in `packages/functions/src/procurement/calculate-inventory-velocity.ts`.
- Data Source: Operates strictly as a read-heavy analytical query against the immutable `stock_movement_ledgers` table to avoid impacting live POS transaction speed.
- Schema Addition (Future Phase): Updates `inventory_velocity_metrics` cache table with suggested vs actual min/max targets for merchant approval.
