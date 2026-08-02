---
type: Reference
title: "02 Permanent Store Credit Ledger"
description: "Knowledge document for 02 Permanent Store Credit Ledger."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.853943Z }
---

# Permanent Store Credit Ledger

## Permanent Non-Expiring Policy Architecture
The store credit architecture implements strict system guardrails to guarantee that store credit balances NEVER expire or forfeit. This permanent, non-expiring policy applies universally to all credits generated from trade-ins, returns, layaway releases, or promotional grants. By removing expiration dates entirely, the system builds customer trust and simplifies liability accounting.

## Immutable Ledger Table (`store_credit_ledger`)
Every store credit transaction (both credits and debits) is recorded in an append-only, immutable ledger. This ensures a complete and indisputable audit trail of all balance changes.

Each ledger entry contains:
- **`amount`**: The value added or subtracted.
- **`balance_after`**: The total available balance immediately following the transaction.
- **`transaction_type`**: Categorization of the transaction. Supported types include:
  - `TRADE_IN_PAYOUT`
  - `RETURN_CREDIT`
  - `REGISTER_REDEMPTION`
  - `SHOPIFY_WEB_REDEMPTION`
  - `PROMOTIONAL_GRANT`
- **`reference_id`**: A pointer to the originating transaction, order, or event.

## Real-Time Shopify Synchronization
To provide a seamless omnichannel experience, store credit balances are synchronized in real-time between the POS system and the online store.
- **Bi-directional Sync**: Utilizes the Shopify Customer Store Credit GraphQL API (`customerStoreCreditCredit` / `customerStoreCreditDebit`) or the Gift Card API to mirror ledger events.
- **Instant Availability**: Guarantees that balance updates are instantaneously reflected, whether a customer spends credit at the physical in-store register or on the Shopify web store.

## Anti-Collision & Concurrency Guard
Given the possibility of a customer attempting to use store credit simultaneously in-store and online, robust concurrency controls are critical.
- **Optimistic Locking**: Ensures that multiple simultaneous redemption requests do not result in a negative balance or double-spending.
- **Database Transactions**: All debit operations are wrapped in strict database transactions. The operation will fail and roll back if the requested redemption amount exceeds the current `balance_after` of the latest ledger entry, ensuring data integrity across all sales channels.
