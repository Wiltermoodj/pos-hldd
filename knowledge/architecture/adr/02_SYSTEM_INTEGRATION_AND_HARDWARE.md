---
type: Concept
title: "02. System Integration & Hardware Architecture"
---

# 02. System Integration & Hardware Architecture

## 1. POS Cart & Component Consolidation

### Rationale & Design Invariant
Having parallel component trees (`components/checkout/` and `components/cart/`) causes state synchronization bugs and doubles maintenance overhead.

### Rules
1. `apps/pos/src/components/cart/` is the **single canonical source of truth** for all POS cart state, item variant selection, discount applications, and payment processing UIs.
2. `apps/pos/src/components/checkout/` is deprecated and marked for deletion.

---

## 2. Hardware Fault Isolation & Print Lifecycles

### Rationale & Design Invariant
Hardware peripherals (thermal receipt printers, cash drawers, barcode scanners) are inherently fault-prone (out of paper, bluetooth disconnect, paper jam). Peripherals must act as **Deep Modules** that absorb operational complexity. Peripheral failures MUST NOT roll back or fail a successfully processed payment or database transaction.

### Rules
1. **Transaction Commit First:** Payment processing and database transaction ledger commits MUST execute and succeed prior to triggering printer or cash drawer hardware routines.
2. **Graceful Fallback UI:** If thermal printing fails, the system MUST catch the hardware error and present a UI banner with options: `[ Retry Print ]`, `[ Email Receipt ]`, `[ Skip ]`.
3. **Global Barcode Listener:** `use-barcode-scanner.ts` MUST capture global scanner keypress sequences while strictly ignoring events when user focus is inside an editable input (`HTMLInputElement`, `HTMLTextAreaElement`).
4. **Cash Drawer Pulse Control:** Sending ESC/POS pulse commands (`ESC p 0 25 250`) MUST be restricted strictly to verified cash/check payment completion or explicit cashier "No Sale" manager overrides logged to audit tables.

---

## 3. Offline-First Queue & Idempotency Engine

### Rationale & Design Invariant
Registers must continue functioning during network disruptions. When connectivity drops, sales queue locally in IndexedDB and sync upon reconnection. Server Actions must enforce strict idempotency to prevent duplicate charges or double stock deductions.

### Rules
1. Every offline sale transaction queued in `offline-queue.ts` MUST generate a client-side `idempotency_key: UUIDv4` before local persistence.
2. Upon network reconnection, Server Actions MUST check the database for the incoming `idempotency_key`. If the key exists, the action MUST return the cached transaction result without re-executing payment processing or ledger deductions.

```typescript
// ✅ MANDATORY: Server Action Idempotency Guard
export async function processSaleAction(input: SaleInput): Promise<ActionResult<SaleResult>> {
  const existing = await db.query.orders.findFirst({
    where: eq(orders.idempotencyKey, input.idempotencyKey)
  });
  if (existing) {
    return { success: true, data: formatOrderResult(existing) };
  }
  // Proceed with new sale execution...
}
