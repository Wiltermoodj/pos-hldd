# Inventory Reservation State Machine & Availability Engine

## 1. Executive Summary
Tracking inventory with a single generic reserved integer creates race conditions and stock drift across multi-channel retail environments. This specification establishes explicit reservation intent buckets to guarantee accurate sellable inventory counts across physical registers and online channels.

## 2. Multi-Location Inventory Level Schema Definition
Every inventory location tracks physical stock alongside four discrete reservation allocations:

- `stocked_quantity` (integer): Total physical units present inside the building.
- `reserved_bopis` (integer): Units requested via online store purchase (Pending Store Confirmation).
- `reserved_special_order` (integer): Units allocated to a vendor Purchase Order for a specific customer.
- `reserved_workshop` (integer): Units allocated to an active Service Work Order ticket.
- `reserved_layaway` (integer): Serialized units held on a customer layaway deposit.

## 3. Real-Time Available Inventory Formula
Both the POS cashier interface and online storefront APIs MUST calculate available sellable stock using this strict formula:

AvailableStock = stocked_quantity - (reserved_bopis + reserved_special_order + reserved_workshop + reserved_layaway)

## 4. State Transitions & Lifecycle Rules
1. E-Commerce Order Placed:
   - Action: Increment `reserved_bopis`.
   - Result: `stocked_quantity` remains unchanged. `AvailableStock` decreases by order quantity.

2. Store Confirms E-Commerce Request:
   - Action: Capture payment authorization hold.
   - Result: Decrement `reserved_bopis`, decrement `stocked_quantity`.

3. Store Declines E-Commerce Request (In-Store Priority / Stock Discrepancy):
   - Action: Void authorization hold ($0 fee incurred).
   - Result: Decrement `reserved_bopis`. `AvailableStock` automatically restores.
