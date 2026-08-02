---
type: Module
resource: apps/pos/src/lib/offline/offline-queue.ts
domain: POS Register
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for offline-queue.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `enqueueTransaction(transaction: Omit<OfflineTransaction, 'id' | 'timestamp' | 'idempotencyKey'>): Promise<OfflineTransaction>`
  - `getQueuedTransactions(): Promise<OfflineTransaction[]>`
  - `clearQueue(): Promise<void>`
  - `removeTransactions(transactionIds: string[]): Promise<void>`
  - `processQueue(): Promise<void>`
  - `initOfflineSync(): void`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
