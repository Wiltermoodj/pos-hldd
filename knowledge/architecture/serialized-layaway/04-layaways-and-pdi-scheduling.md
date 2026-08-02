---
type: Reference
title: "04 Layaways And Pdi Scheduling"
description: "Knowledge document for 04 Layaways And Pdi Scheduling."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.857376Z }
---

# Layaways and PDI Scheduling

## Layaway Deposit & Milestone Installments
The system supports flexible payment schedules for layaway items. Key features include:
- Initial deposits (e.g., 20% down) processed via the Stripe Terminal.
- Bi-weekly automated reminders for subsequent milestone installments.
- Explicit, configurable rules for cancellation and restocking fees.

## Physical Storage Rack Tagging
To effectively manage physical space and ensure easy retrieval, the system incorporates storage location tagging for layaway items.
- A `rack_location_tag` is assigned to each layaway (e.g., "Rack 2A - Layaway Tag #LAY-104").
- The system generates and prints thermal handlebar tags to easily identify the physical item in the storage area.

## Pre-Delivery Inspection (PDI) Workshop Scheduler
The system seamlessly integrates layaways with the workshop schedule to ensure a smooth handover process.
- **Automated Scheduling:** Setting a target pickup date for a layaway automatically schedules a 30-minute mechanic PDI/Assembly slot on the workshop calendar.
- **Timing:** This slot is typically scheduled 24-48 hours prior to the customer's pickup.
- **Preparation:** This process ensures the bicycle is unboxed, assembled, tuned, and safety-inspected *before* the customer arrives, maximizing efficiency and customer satisfaction.

## Layaway Expiration & Floor Release
To prevent inventory from being tied up indefinitely, the system includes automated expiration management.
- **Automated Alerts:** Warning alerts are triggered when layaways approach or exceed their defined expiration thresholds.
- **Manager Approval:** The system prompts for manager approval to take action on expired layaways, such as:
  - Releasing the stock back to the sales floor.
  - Converting the customer's initial deposit to store credit, in accordance with the cancellation policy.