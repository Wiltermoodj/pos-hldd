---
type: Reference
title: "03 Vendor Shipping Restrictions And Map"
description: "Knowledge document for 03 Vendor Shipping Restrictions And Map."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.854854Z }
---

# Vendor Shipping Restrictions and MAP

## Vendor Territory & Shipping Restriction Engine
The system includes configurable rules per vendor or brand to enforce vendor-mandated "In-Store Pickup Only" rules. For example: `brand: "Specialized"` -> `shipping_restriction: "IN_STORE_PICKUP_ONLY"`.

## Automated Shopify Delivery Profile Tagging
To enforce these shipping restrictions automatically:
- The system automatically pushes `restrict_shipping_pickup_only` tags to Shopify products upon sync.
- It assigns restricted variants to Shopify's Pickup-Only Delivery Profile via the GraphQL `deliveryProfileUpdate` mutation.
- This effectively blocks online home delivery rates at the Shopify web checkout for complete bikes or restricted components.

## Minimum Advertised Price (MAP) Protection
The POS system enforces strict Minimum Advertised Price (MAP) protection:
- It ensures that published web prices never dip below the `vendor_map_price`.
- It alerts the shop manager if an in-store discount rule attempts to push web prices below vendor MAP thresholds.
