---
type: Reference
title: "05 Receiving Landed Cost And Reconciliation"
description: "Knowledge document for 05 Receiving Landed Cost And Reconciliation."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.863072Z }
---

# Receiving, Landed Cost, and Reconciliation

## Touchscreen Receiving UI Layout

The receiving interface is designed for high-throughput back-of-house operations utilizing standard 2D barcode scanners and touchscreen tablets:

- **Hardware Scanner Loop:** The React UI listens globally for rapid keyboard-emulation scanner inputs.
- **Audio/Visual Cues:**
  - **Success (Standard Item):** Short, high-pitched chime; UI flashes green.
  - **Success (Special Order):** Distinct, multi-tone alert; UI flashes yellow/orange; receipt printer triggers reserve tag.
  - **Error (Unknown Barcode / Over-receiving):** Harsh, low-pitched buzz; UI flashes red; modal blocks further scanning until acknowledged.
- **Multi-Pack Resolution:** When a master carton barcode is scanned, the UI automatically prompts the user to verify if they are receiving the master carton (e.g., box of 10 tubes) or a single inner unit, adjusting the received quantity accordingly.

## Landed Cost Allocation Engine

Accurately calculating margin requires understanding the "True" Landed Cost of an item, which includes not just the wholesale price, but proportional freight and processing fees.

### Value-Based Allocation Formula

The default and most equitable method for mixed retail goods is value-based allocation. It prevents cheap, lightweight items (like inner tubes) from absorbing the same freight costs as expensive items (like derailleurs).

$$ \text{Landed Cost Per Unit}_i = \text{Invoiced Cost}_i + \left( \frac{\text{Line Invoiced Value}_i}{\text{Total GRN Value}} \times \frac{\text{Total Freight + Fees}}{\text{Qty Received}_i} \right) $$

*Where:*
- *Line Invoiced Value* = Invoiced Cost × Qty Received
- *Total GRN Value* = Sum of all Line Invoiced Values in the shipment

### Weight-Based Allocation Formula

For shipments consisting of uniformly priced but vastly different weighted items (e.g., lead acid batteries vs. carbon bottle cages), a weight-based formula is available if dimensional data exists in the catalog.

### TypeScript Implementation

```typescript
/**
 * Calculates the allocated landed cost per unit for a specific line item
 * using the Value-Based Allocation method.
 */
export function calculateLandedCostValueBasis(
  itemInvoicedCostCents: number,
  itemQtyReceived: number,
  totalGrnValueCents: number,
  totalFreightAndFeesCents: number
): number {
  if (totalGrnValueCents <= 0 || itemQtyReceived <= 0) {
    return itemInvoicedCostCents; // Fallback to base cost if data is invalid
  }

  const lineInvoicedValueCents = itemInvoicedCostCents * itemQtyReceived;

  // Calculate the proportion of the total GRN value this line represents
  const valueProportion = lineInvoicedValueCents / totalGrnValueCents;

  // Allocate total freight/fees based on that proportion
  const allocatedFeesForLineCents = totalFreightAndFeesCents * valueProportion;

  // Determine the per-unit fee addition
  const perUnitFeeCents = allocatedFeesForLineCents / itemQtyReceived;

  // Return the final landed cost per unit (rounded to 2 decimal places)
  return Math.round(itemInvoicedCostCents + perUnitFeeCents);
}
```

## Invoice Variance Reconciliation

When the physical vendor invoice arrives (often after the goods), discrepancies between expected PO costs and actual invoiced costs must be reconciled.

- **Manager Approval Rules:** If the invoiced unit cost exceeds the expected PO unit cost by **> 3%**, the system flags the GRN line item. It requires manual manager authorization to accept the variance before the inventory value is officially updated.
- **Moving Average Cost (MAC):** Upon final reconciliation, the system updates the global internal catalog cost using a Moving Average Cost formula:
  $$ \text{New MAC} = \frac{(\text{Current Qty} \times \text{Current MAC}) + (\text{Received Qty} \times \text{Landed Cost})}{(\text{Current Qty} + \text{Received Qty})} $$

## Discrepancy & RMA Claims

The receiving module includes explicit workflows for handling vendor errors:
- **Short-Shipments:** If `Qty Received` < `Qty Ordered` and the vendor marks the PO complete (no backorder), the system generates a "Shortage Claim" report to be submitted to the vendor's AP department.
- **Defective Goods (RMA):** If goods arrive damaged, the user scans them into a temporary `QUARANTINE` status. The UI prompts for photo capture and initiates a Vendor Return Merchandise Authorization (RMA) workflow, holding the inventory out of available sale stock until the vendor issues credit.