---
type: Reference
title: "apps/pos/src/lib/offline/offline-queue.ts"
description: "Provides domain functionality for offline-queue."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.850394Z }
tags: ['pos-register']
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
