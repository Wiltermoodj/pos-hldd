---
type: Reference
title: "05 Data Model And Database Schema"
description: "Knowledge document for 05 Data Model And Database Schema."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.855045Z }
---

# Data Model and Database Schema

## Production Drizzle ORM Schema (`schema/shopify_sync.ts`)
The production schema leverages Drizzle ORM to maintain data integrity and mapping definitions.

- `shopify_sync_configs`: Stores access tokens, shop domain, webhook secrets, default safety buffers, and sync enabled toggles.
- `shopify_product_mappings`: Links the local `product_id` to the `shopify_product_id`. Tracks sync status (`SYNCED`, `PENDING`, `ERROR`) and the last synced timestamp.
- `shopify_variant_mappings`: Links the local `variant_id` to the `shopify_variant_id` and `shopify_inventory_item_id`. It also stores the barcode, synced price, and synced stock.
- `shopify_sync_queue`: Manages the queue with fields for Queue ID, entity type (`PRODUCT`, `VARIANT`, `INVENTORY`), payload JSON, retry count, and error message.
- `shopify_webhook_logs`: Logs incoming webhooks including Webhook ID, topic (`orders/create`, etc.), payload, and processing status.
- `bopis_fulfillment_holds`: Manages BOPIS holds with fields for Order ID, Shopify order ID, customer name, items JSON, pick status (`PENDING_PICK`, `PICKED_STAGED`, `PICKED_UP`), and rack location tag.

## Indexes & Foreign Keys
The database schema utilizes robust indexing and constraints:
- **Unique Indexes**: Applied on Shopify IDs to prevent duplication.
- **Composite Indexes**: Used on sync queues for performant querying of pending sync events.
- **Foreign Keys**: Enforces relational integrity with foreign key constraints to core products.
