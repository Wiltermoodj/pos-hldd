# Workshop Scheduling & Mechanic Capacity

## Labor-Hour Capacity Calendar
To optimize workshop throughput and prevent mechanic burnout, scheduling is managed via a visual Labor-Hour Capacity Calendar.

- **Visual Bounds:** Scheduling uses drag-and-drop mechanics to allocate bounds based on standardized labor hours (e.g., an "Annual Tune" blocking out 1.5 hours).
- **Load Balancing:** The system performs real-time load balancing, checking individual mechanic availability and skill sets to prevent over-booking on any given day.

## Work Order Status Pipeline
Work orders progress through a strictly defined state machine, ensuring visibility across the workshop and sales floor.

- **Status Flow:** `INTAKE` -> `QUEUED` -> `IN_PROGRESS` -> `AWAITING_PARTS` -> `AWAITING_ESTIMATE_APPROVAL` -> `READY_FOR_PICKUP` -> `COMPLETED`.
- **Transitions:** Each state transition triggers relevant automated actions, such as customer notifications or updating parts reservation status.

## Workshop Touchscreen Terminal Mode
A specialized, distraction-free user interface designed specifically for mechanics operating at their benches.

- **Dedicated UI:** The Workshop Touchscreen Terminal Mode focuses on the active work order.
- **Barcode Integration:** Mechanics can scan replacement parts barcodes directly into the active work order. This live inventory deduction is critical to eliminate phantom inventory loss and ensure accurate billing.

## Mechanic Time Tracking
Accurate labor tracking is built into the workflow to measure workshop efficiency and validate service package pricing.

- **Clock-In / Clock-Out:** Mechanics log their time (clock-in/clock-out) per specific work order line item.
- **Analytics:** This data is used to measure actual labor hours against estimated labor hours per service type, providing actionable insights for business management.
