# Open Knowledge Architecture Manifest

## Core Domains
- **pos-register**: Core point-of-sale functionality, barcode scanning, printing, local queues.
- **service-workshop**: Workorders, repair tags, bike assets, availability scheduling.
- **ordering**: Checkouts, Shopify synchronization, webhooks, online order requests.
- **inventory-ledger**: Inventory stock levels, product catalog, bopis/special-order reservations, stock movements.
- **procurement**: Purchase orders, vendor distribution APIs, cost calculation.

## Global Architectural Rules
- **Integer Cents**: All financial amounts must be represented and stored as integer cents to prevent floating-point precision issues.
- **ActionResult<T> Wrappers**: Server Actions must return standardized wrappers `{ success: true, data: T }` or `{ success: false, error: string }`.
- **Zero-Knowledge Privacy Boundaries**: Systems should not log or improperly expose raw PII or secret API keys, relying on sanitized telemetry.
- **Location Isolation**: Almost all records must be scoped to a `locationId` to support multi-tenant/multi-location isolation natively.
- **ISO-8601 UTC**: All date/time storage and APIs must use ISO-8601 UTC format.
