---
type: Reference
title: "04 Sms Estimate Approvals And Hang Tags"
description: "Knowledge document for 04 Sms Estimate Approvals And Hang Tags."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.858808Z }
---

# SMS Estimate Approvals & Hang Tags

## Interactive SMS Quote Approval Engine
During service, mechanics frequently discover unexpected repairs needed beyond the original estimate. The Interactive SMS Quote Approval Engine streamlines obtaining customer authorization for these additional costs.

- **Triggering Estimates:** When a mechanic adds unexpected repair items (e.g., a worn chain and a bent derailleur hanger), they trigger the `send_estimate_sms` action.
- **Dispatch:** The system utilizes the Twilio API to dispatch an SMS containing a unique, secure token link (e.g., `/service-quote/[token]`) to the customer's mobile device.
- **Mobile Web Approval Page:** The link opens a mobile-optimized web page where the customer can view the suggested additions and simply tap checkmarks to accept or decline individual recommended line items.
- **Real-Time Work Order Update:** When items are accepted, the system immediately attaches them to the active work order, recalculates the outstanding balance, and automatically reserves the required parts in inventory. The work order status may also automatically transition back to `IN_PROGRESS` from `AWAITING_ESTIMATE_APPROVAL`.

## Handlebar Hang-Tag Printing Driver
Efficient physical organization within the workshop is managed via automated hang-tags attached directly to the bikes.

- **Printing Technology:** The system utilizes an ESC/POS thermal printer driver tailored for formatting byte streams to hardware printers.
- **Tag Details:** The driver renders weather-resistant 2"x4" handlebar tags. These tags include critical information for quick identification:
  - Work Order # (rendered as a scannable barcode)
  - Customer Name
  - Bike Serial Number (and brief description)
  - Promised Pickup Date
  - Rack Location Tag
