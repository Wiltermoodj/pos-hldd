---
type: Reference
title: "03 Data Model And Database Schema"
description: "Knowledge document for 03 Data Model And Database Schema."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.862269Z }
---

# Data Model and Database Schema

## Entity-Relationship Overview

The foundation of the procurement module relies on a tightly integrated relational data model that connects vendors to internal products, and tracks the complete lifecycle of inventory acquisition from Purchase Order (PO) to Goods Received Note (GRN), tightly binding special customer orders along the way.

- **Vendors** define the supplier entity.
- **Vendor Item Mappings** create the bridge between our local internal product variants and the vendor-specific SKUs and pricing.
- **Purchase Orders** aggregate mapped items intended for acquisition.
- **Goods Received Notes (GRNs)** log the physical intake of items, allocating freight and fees to determine true landed cost.
- **Special Order Links** tie an incoming POS customer order line item directly to an outbound PO line item, ensuring the customer's reserved unit is tracked through receiving.

## Production Drizzle ORM Schema (`schema/procurement.ts`)

The PostgreSQL database is accessed and managed using Drizzle ORM. Below is the production schema definition:

```typescript
import {
  pgTable,
  serial,
  text,
  integer,
  timestamp,
  jsonb,
  boolean,
  uniqueIndex,
  index
} from 'drizzle-orm/pg-core';
import { relations } from 'drizzle-orm';

// --- VENDORS ---
export const vendors = pgTable('vendors', {
  id: serial('id').primaryKey(),
  code: text('code').notNull().unique(), // e.g., 'QBP', 'SHIMANO'
  name: text('name').notNull(),
  accountNumber: text('account_number'),
  freeFreightThresholdCents: integer('free_freight_threshold_cents'),
  encryptedCredentials: jsonb('encrypted_credentials'), // API Keys/OAuth tokens
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
});

// --- VENDOR ITEM MAPPINGS ---
export const vendorItemMappings = pgTable('vendor_item_mappings', {
  id: serial('id').primaryKey(),
  vendorId: integer('vendor_id').references(() => vendors.id).notNull(),
  internalVariantId: text('internal_variant_id').notNull(), // Ref to core product catalog
  vendorSku: text('vendor_sku').notNull(),
  vendorUpc: text('vendor_upc'),
  wholesaleCostCents: integer('wholesale_cost_cents').notNull(),
  isPrimary: boolean('is_primary').default(false), // Primary supplier for reorders
}, (table) => ({
  mappingUnique: uniqueIndex('vendor_internal_unique').on(table.vendorId, table.internalVariantId),
  vendorSkuIdx: index('vendor_sku_idx').on(table.vendorSku),
  upcIdx: index('vendor_upc_idx').on(table.vendorUpc),
}));

// --- PURCHASE ORDERS ---
export const purchaseOrders = pgTable('purchase_orders', {
  id: serial('id').primaryKey(),
  vendorId: integer('vendor_id').references(() => vendors.id).notNull(),
  status: text('status').notNull().default('DRAFT'), // DRAFT, SUBMITTED, PARTIAL, FULFILLED, CANCELLED
  subtotalCents: integer('subtotal_cents'),
  estimatedFreightCents: integer('estimated_freight_cents'),
  submittedAt: timestamp('submitted_at'),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
}, (table) => ({
  statusIdx: index('po_status_idx').on(table.status),
  vendorPoIdx: index('po_vendor_idx').on(table.vendorId),
}));

export const purchaseOrderItems = pgTable('purchase_order_items', {
  id: serial('id').primaryKey(),
  purchaseOrderId: integer('purchase_order_id').references(() => purchaseOrders.id).notNull(),
  vendorItemMappingId: integer('vendor_item_mapping_id').references(() => vendorItemMappings.id).notNull(),
  quantityOrdered: integer('quantity_ordered').notNull(),
  quantityReceived: integer('quantity_received').default(0),
  unitCostCents: integer('unit_cost_cents').notNull(),
});

// --- GOODS RECEIVED NOTES (GRN) ---
export const goodsReceivedNotes = pgTable('goods_received_notes', {
  id: serial('id').primaryKey(),
  purchaseOrderId: integer('purchase_order_id').references(() => purchaseOrders.id).notNull(),
  carrier: text('carrier'),
  trackingNumber: text('tracking_number'),
  invoiceReference: text('invoice_reference'),
  actualFreightCents: integer('actual_freight_cents'),
  additionalFeesCents: integer('additional_fees_cents'),
  receivedAt: timestamp('received_at').defaultNow(),
});

export const grnItems = pgTable('grn_items', {
  id: serial('id').primaryKey(),
  grnId: integer('grn_id').references(() => goodsReceivedNotes.id).notNull(),
  purchaseOrderItemId: integer('po_item_id').references(() => purchaseOrderItems.id).notNull(),
  quantityReceived: integer('quantity_received').notNull(),
  allocatedLandedCostCents: integer('allocated_landed_cost_cents'),
});

// --- SPECIAL ORDER LINKS ---
export const specialOrderLinks = pgTable('special_order_links', {
  id: serial('id').primaryKey(),
  posLineItemId: text('pos_line_item_id').notNull(), // Ref to POS receipt line
  customerId: text('customer_id').notNull(),
  depositAmountCents: integer('deposit_amount_cents').notNull(),
  purchaseOrderItemId: integer('purchase_order_item_id').references(() => purchaseOrderItems.id),
  status: text('status').notNull().default('QUEUED_FOR_PO'),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
}, (table) => ({
  posLineItemUnique: uniqueIndex('pos_line_item_idx').on(table.posLineItemId),
  soStatusIdx: index('so_status_idx').on(table.status),
}));

// --- RELATIONS ---
export const vendorsRelations = relations(vendors, ({ many }) => ({
  mappings: many(vendorItemMappings),
  purchaseOrders: many(purchaseOrders),
}));

export const purchaseOrderRelations = relations(purchaseOrders, ({ one, many }) => ({
  vendor: one(vendors, {
    fields: [purchaseOrders.vendorId],
    references: [vendors.id],
  }),
  items: many(purchaseOrderItems),
  grns: many(goodsReceivedNotes),
}));
```