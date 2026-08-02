---
type: Reference
title: "01_Domain_And_Financial_Specifications"
description: "Knowledge document for 01_Domain_And_Financial_Specifications."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.861605Z }
---

# 01. Domain & Financial Specifications

## 1. Monetary Precision & Currency Handling

### Rationale & Design Invariant
Floating-point arithmetic in JavaScript/TypeScript causes rounding errors (e.g., `0.1 + 0.2 === 0.30000000000000004`), leading to balance discrepancies, accounting drift, and hardware receipt mismatch. We define currency rounding errors out of existence by mandating that **all internal calculations, domain methods, data models, APIs, and drivers pass monetary values as positive integer cents**.

### Boundary Isolation Rule
Division by 100 or conversion to a decimal string (e.g., `.toFixed(2)`) is strictly isolated to the **final visual/text rendering boundary** (e.g., React DOM elements or ESC/POS print string assembly). No intermediate calculations may use float inputs.

### Code Contracts

```typescript
// ❌ FORBIDDEN: Floating-point parameters and intermediate arithmetic
function calculateTax(price: number, taxRate: number): number {
  return price * taxRate; // Produces float rounding drift
}

// ✅ MANDATORY: Integer cents interface and exact integer arithmetic
function calculateTaxCents(priceCents: number, taxRateBps: number): number {
  // taxRateBps = basis points (e.g., 875 for 8.75%)
  return Math.round((priceCents * taxRateBps) / 10000);
}
2. Universal Date & Time Engineering
Rationale & Design Invariant
To prevent timezone drift and order history ambiguities across PostgreSQL, Server Actions, React clients, and physical store receipts, all timestamps must represent absolute UTC instants at storage and transfer boundaries.

Rules
Database Layer: All timestamp columns in Drizzle schemas must use timestamp with time zone or ISO-8601 UTC strings. Standard column names: created_at, updated_at, completed_at.

API & Action Boundaries: Timestamps passed across network or server boundaries MUST be valid ISO-8601 UTC strings (e.g., 2026-07-31T23:54:32.000Z).

Integration Adapters: External epoch timestamps (e.g., Stripe's Unix epoch seconds) MUST be converted to ISO UTC strings immediately at the integration adapter boundary (src/lib/stripe-terminal/).

Display & Hardware Boundary: Conversion from UTC ISO strings to local display strings MUST happen strictly at the final display boundary using the physical store location's configured timezone (e.g., America/Los_Angeles), NOT the client device's browser timezone.

TypeScript
// ❌ FORBIDDEN: Passing unformatted local strings or raw un-suffixed integers
const payload = { createdAt: new Date().toString() };

// ✅ MANDATORY: ISO UTC string transfer boundary
const payload = { createdAt_utc: new Date().toISOString() };
3. Multi-Store Location Context & Session Isolation
Rationale & Design Invariant
To prevent cross-location stock leakage and unauthorized multi-store edits, every POS session is explicitly bound to a location_id and register_id.

Rules
Every active POS register session context MUST be stored in secure, signed HTTP-only cookies or encrypted session storage.

All inventory Server Actions and database queries MUST validate cashier location permissions and explicitly include where(eq(inventoryLevels.locationId, currentLocationId)).

TypeScript
// ❌ FORBIDDEN: Unscoped inventory query
const stock = await db.select().from(inventoryLevels).where(eq(inventoryLevels.productId, productId));

// ✅ MANDATORY: Location-isolated inventory query
const stock = await db.select().from(inventoryLevels).where(
  and(
    eq(inventoryLevels.productId, productId),
    eq(inventoryLevels.locationId, sessionContext.locationId)
  )
);
