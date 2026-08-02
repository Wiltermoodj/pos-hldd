# Oversell Protection and BOPIS Pipeline

## Location Safety Stock Buffer Formula
To guard against overselling, the system applies a location-level safety stock buffer. The formula used is:
$$\text{Shopify Published Available Qty} = \max(0, \text{In-Store Physical Qty} - \text{Safety Buffer Qty})$$

## Synchronous Last-Unit Inventory Lock Protocol
To eliminate race conditions for high-ticket items:
- When a POS register clerk scans the final in-stock unit of a high-ticket item, the POS sends a synchronous `inventorySetQuantities` GraphQL mutation setting Shopify available stock to `0` prior to tender completion.

## POS Register BOPIS Pickup Pipeline
The system includes an automated pipeline for Buy Online Pick Up In Store (BOPIS) orders:
- **Inbound Webhook**: Triggers on an inbound Shopify Webhook (`orders/create` with `fulfillment_service: "manual"` and local pickup location).
- **Alert**: Pushes a visual and audible alert banner to the POS register UI.
- **Pick-Slip Printing**: Provides 1-Click thermal pick-slip printing, rendering a 2"x4" thermal tag with Order #, Customer Name, Items, Bin Location, and Storage Tag.
- **Inventory Hold**: Transitions the item status to `RESERVED_FOR_BOPIS` and releases the hold upon customer pickup via the `fulfillmentOrderReleaseHold` GraphQL mutation.
