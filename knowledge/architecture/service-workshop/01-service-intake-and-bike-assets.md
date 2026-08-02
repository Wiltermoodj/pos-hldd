---
type: Reference
title: "01 Service Intake And Bike Assets"
description: "Knowledge document for 01 Service Intake And Bike Assets."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.857836Z }
---

# Service Intake & Bike Assets Architecture

## Bike Asset Selector & Intake Engine
The Service Intake module serves as the primary entry point for managing repair services. Every work order must be explicitly linked to a `bike_asset` record to ensure proper tracking and continuity of service history.

- **Asset Linking & Creation:** Staff can quickly search and link a work order to an existing customer `bike_asset`. If the bike does not exist in the system, a new asset is created during the intake process.
- **Captured Asset Data:** Comprehensive details are recorded, including:
  - Frame Serial Number (primary identifier)
  - Brand and Model
  - Year of Manufacture
  - Color
  - E-Bike System Details (Motor IDs, Battery IDs, firmware versions)

## Service History Log
An unbroken service history log is maintained for every `bike_asset` registered in the system. This historical context is critical for mechanics to make informed decisions and for customers to track their bike's maintenance.

- **Historical Views:** Mechanics and intake staff can view past repair work orders associated with the bike.
- **Detailed Records:** The log includes all parts replaced, specific mechanic notes and diagnostics from previous services, and total lifetime spend on the specific bike.

## Tiered Service Package Estimator
To streamline the intake process and ensure standardized service levels, the system supports a Tiered Service Package Estimator.

- **Standardized Packages:** Offers predefined tune-up packages such as "Safety Check", "Annual Tune", or "Overhaul".
- **Auto-expansion:** Selecting a package automatically expands it into the required labor line items and suggests bundled replacement parts kits, ensuring consistent pricing and reducing manual entry errors.

## Touchscreen Bike Damage Inspection Diagram
Protecting the retailer from unwarranted liability regarding pre-existing damage is handled via the interactive damage inspection tool.

- **Interactive UI:** A touchscreen-optimized visual bike diagram.
- **Damage Marking:** Allows intake staff to tap and explicitly mark pre-existing scratches, dents, or carbon cracks directly on the diagram prior to finalizing the intake. This information is saved as part of the work order's intake documentation.
