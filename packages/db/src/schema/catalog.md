---
type: Module
resource: packages/db/src/schema/catalog.ts
domain: Inventory Ledger
status: Implemented
---


## Purpose & Responsibilities
- Defines database schema and invariants for catalog.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - None explicitly exported / internal definitions
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Strict typing and relationships using Drizzle ORM.
- Enforces schema level constraints.

## Agreed-Upon Future Goals & Wishlist
- Enhance indices for read-heavy operations.
- Add database-level audit trails.
