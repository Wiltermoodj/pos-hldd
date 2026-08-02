---
type: Reference
title: "01 Overview And Requirements"
description: "Knowledge document for 01 Overview And Requirements."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.862849Z }
---

# Overview and Requirements

## Executive Summary

The POS Procurement & Distributor Ordering Module is designed to bridge the gap between legacy bicycle point-of-sale (POS) features—historically provided by platforms like Ascend and Workstand—and modern e-commerce paradigms exemplified by Shopify POS. By integrating live distributor inventory, automated purchasing workflows, and seamless special order handling into a unified modern tech stack, this module eliminates operational silos, streamlines purchasing, and enables real-time omnichannel synchronization.

## Competitive Gap Analysis Matrix

The following matrix compares our modern architecture against industry standards across key dimensions:

| Feature Dimension | Ascend | Lightspeed | Workstand | Shopify POS (Stocky) | Our Modern Architecture |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Catalog Architecture** | Legacy / Closed | Static Mappings | Fragmented | Basic (e-commerce focused) | Dynamic, multi-vendor relational mapping |
| **Live Vendor Stock Checks** | Proprietary integrations | Limited / Periodic | Dependent on static feeds | None out-of-the-box | Real-time concurrent API streaming |
| **Special Order Binding** | Strong | Moderate | Weak | Workarounds needed | First-class, lifecycle-bound (POS to PO) |
| **Landed Cost** | Average | Basic | Basic | Simple average | Advanced value/weight-based dynamic allocation |
| **Integration Flexibility** | Low | Moderate | Low | High (Apps) | High (Adapter pattern for any B2B/EDI feed) |

## Technical Alignment

The module's architecture maps core operational requirements to a modern, scalable technology stack:

- **Next.js (App Router):** Provides a robust serverless environment and server actions for business logic, hosted on Vercel.
- **Neon PostgreSQL:** A serverless, auto-scaling relational database layer tailored for fast connections.
- **Drizzle ORM:** Typesafe, lightweight database querying optimized for edge execution.
- **Touchscreen React POS UI:** A fast, Tailwind-driven interface designed specifically for touchscreen registers with custom hardware hooks (barcode scanners, receipt printers).
- **Stripe Terminal SDK:** Manages in-person split payments and specialized workflows like partial deposits for special orders.
- **Shopify GraphQL API:** Enables bidirectional synchronization for omnichannel orders, inventory matrices, and BOPIS (Buy Online, Pick Up In Store) state management.

## Non-Functional Requirements

To ensure optimal performance in a fast-paced retail environment, the module is engineered against the following non-functional targets:

- **Latency:**
  - Local catalog search queries must resolve in **< 50ms**.
  - Live vendor API stock check streaming must render results in **< 300ms**.
- **Throughput:**
  - The receiving hardware loop must comfortably support **> 60 scans/min** to facilitate rapid Goods Received Note (GRN) intake without UI blocking.
- **Data Integrity:**
  - Strict data ledger immutability, especially concerning Purchase Orders (POs) and Landed Cost allocation updates.
- **Resilience:**
  - Designed for intermittent offline resilience to ensure critical register functions and queued PO tasks do not block immediate point-of-sale customer interactions.