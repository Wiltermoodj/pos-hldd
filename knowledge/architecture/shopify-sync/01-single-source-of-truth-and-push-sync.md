---
type: Reference
title: "01 Single Source Of Truth And Push Sync"
description: "Knowledge document for 01 Single Source Of Truth And Push Sync."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.855653Z }
---

# Single Source of Truth and Push Sync Architecture

## POS Single Source of Truth Architectural Rule
The POS system is the absolute single source of truth for all catalog items, variants, options, prices, MSRPs, barcodes, and inventory counts. All product metadata (Title, Description, Category, Brand, Price, Cost, MSRP, Barcode, Images) and Inventory Quantities are strictly managed in the POS database. Edits in Shopify are overwritten by POS ground truth.

## One-Way Push Sync Architecture (POS -> Shopify)
The synchronization flows strictly from POS to Shopify via a one-way push architecture.
- Any database update (Insert/Update/Delete) on `products`, `product_variants`, or `inventory_levels` queues an event in `shopify_sync_queue`.
- A background worker pushes these changes to Shopify via the Admin GraphQL API.

## Shopify Webhook Role
Shopify webhooks strictly serve inbound order ingestion (`orders/create`, `refunds/create`), BOPIS fulfillment holds, and customer sync. Direct product/variant edits on Shopify trigger an automated reconciliation check that re-asserts POS ground truth.

## Sync Log & Audit Trail
Every GraphQL request/response, latency, and status is logged in `shopify_sync_logs` to maintain a robust audit trail of all synchronization activities.
