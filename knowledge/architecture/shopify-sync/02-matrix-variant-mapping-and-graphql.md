---
type: Reference
title: "02 Matrix Variant Mapping And Graphql"
description: "Knowledge document for 02 Matrix Variant Mapping And Graphql."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.855228Z }
---

# Matrix Variant Mapping and GraphQL Mutations

## Multi-Option Variant Matrix Mapping
There is a standardized mapping between the POS variant matrix structure and Shopify GraphQL `ProductInput` / `ProductVariantInput`. This ensures full support for multi-option variant matrixes with zero duplicate products or broken barcode mappings.

## Support for 3 Option Dimensions
The system supports up to 3 option dimensions for variants:
- **Option 1: Size** (e.g., 52cm, 54cm, 56cm, S, M, L)
- **Option 2: Color** (e.g., Matte Black, Gloss Red)
- **Option 3: Spec / Build** (e.g., Shimano 105, SRAM AXS)

## Shopify Admin GraphQL Mutations
The synchronization uses the following Shopify Admin GraphQL Mutations:
- `productCreate` & `productUpdate`: Used for syncing parent product metadata.
- `productVariantsBulkCreate` & `productVariantsBulkUpdate`: Used for high-performance variant pushes.
- `inventorySetQuantities` & `inventoryAdjustQuantities`: Used for precision stock updates.

## Deterministic SKU/UPC Alignment
To prevent variant duplication, a strict 1:1 mapping is maintained between `product_variants.id` and `shopify_variant_id`. These are securely linked by immutable `vendor_upc` / `sku` identifiers.
