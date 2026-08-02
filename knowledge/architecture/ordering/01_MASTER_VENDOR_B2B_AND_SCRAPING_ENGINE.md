---
type: Reference
title: "01_Master_Vendor_B2B_And_Scraping_Engine"
description: "Knowledge document for 01_Master_Vendor_B2B_And_Scraping_Engine."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.863488Z }
---

# Master B2B Vendor Aggregation & Live Scraping Engine

- **Master Vendor Order Screen**: Showing real-time in-stock status and wholesale cost across all vendor accounts simultaneously.
- **Dual Ingestion Architecture**: Direct B2B API integrations (QBP, Shimano, BTI, JBI) combined with headless web scrapers (Playwright) for non-API dealer portals.
- **Secure B2B Credential Vault**: Storing encrypted login sessions, cookies, and schemas for automated headless logins.
- **Dynamic Order Cutoff Optimization**: Ingesting daily vendor order cutoff times (e.g., 3:00 PM PST dispatch) to automatically suggest the fastest vendor to meet workorder promise dates.
