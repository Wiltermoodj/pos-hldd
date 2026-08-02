---
type: Concept
title: "Departmental Loyalty Rewards"
---

# Departmental Loyalty Rewards

## Departmental Earning Multipliers
The loyalty rewards program features a configurable point calculation engine that applies variable earning rates based on the specific product or service department of each line item. This allows the business to incentivize high-margin categories (like service labor) differently than low-margin categories (like complete bikes).

The formula for calculating points earned on a transaction is:

$$\text{Points Earned} = \sum \left( \text{Line Item Amount}_i \times \text{Department Multiplier}_i \right)$$

### Example Default Multipliers:
- **Service Labor & Parts:** 2.0x
- **Apparel & Accessories:** 1.0x
- **Complete Bikes:** 0.25x
- **Gift Cards / Deposits:** 0.0x

## Reward Tier Engine
Customers are segmented into tiers based on their lifetime points earned or other engagement metrics. The Reward Tier Engine manages these classifications and the associated benefits.

Example tiers include:
- **Bronze Rider:** Entry-level tier, standard point earning.
- **Silver Rider:** Mid-level tier, unlocks perks such as free annual safety checks.
- **Gold Rider:** Top-tier status, unlocks premium perks like priority workshop scheduling and bonus point multipliers across all departments.

## Redemption Protocol
The redemption protocol governs how accumulated loyalty points are converted into spendable value.
- **Flexible Conversion:** Points can be converted to dollar value (Store Credit) at both the POS register and the online checkout.
- **Configurable Thresholds:** The system enforces minimum redemption thresholds to encourage accumulation before spending. For example, a standard configuration might be **500 points = $10 Store Credit**.
