---
type: Concept
title: "Trade-in and Refurbishment"
---

# Trade-in and Refurbishment

## Uncluttered POS UI Architecture
Since trade-ins are an uncommon transaction type, the UI is designed to prioritize speed for normal checkouts. The trade-in intake workflow utilizes a secondary modal or slide-over pattern, accessible via "Register Actions -> Trade-In Intake". This ensures the main register UI remains 100% focused on rapid item scanning, uncluttered by trade-in specific fields and workflows.

## Bicycle Blue Book (BBB) API Integration
The system integrates directly with the Bicycle Blue Book (BBB) B2B API to provide real-time, accurate valuations for trade-in bicycles. Staff can retrieve trade-in valuation tiers (Private Range, Trade-in Value, MSRP) by selecting the following parameters in sequence:
1. Year
2. Brand
3. Model
4. Trim
5. Condition Grade (Fair, Good, Very Good, Excellent)

## Trade-In Valuation Matrix
Based on the BBB valuation and store policies, the system calculates a dual offering for the customer:
- **Store Credit Allowance:** A higher value designed to keep the value within the business and incentivize immediate purchase.
- **Cash/Payout Amount:** A lower value offered for customers who prefer cash out over store credit.

## Refurbishment Work Order Auto-Creation
Upon accepting a trade-in, the system automatically creates a linked Service Workshop Work Order. This work order facilitates the scheduling and tracking of tune-up parts and labor (e.g., new chain, fresh bar tape, safety tune). The total refurbished cost basis is automatically calculated using the following formula:

$$\text{Used Bike Cost Basis} = \text{Trade-In Allowance} + \text{Refurb Parts Wholesale Cost} + \text{Mechanic Labor Cost}$$

## Transition to Used Sellable Stock
Once the refurbishment work order is marked as complete, the system automates the transition of the bicycle into used sellable stock. This process includes:
- Automated creation of a unique `used_bike_variant` in the inventory system.
- Generation of a barcode for the refurbished item to allow for scanning and sale at the register.
