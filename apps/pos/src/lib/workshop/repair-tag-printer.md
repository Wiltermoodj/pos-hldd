---
type: Module
resource: apps/pos/src/lib/workshop/repair-tag-printer.ts
domain: Service Workshop
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for repair-tag-printer.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `executeReceiptPrint(workOrder: RepairWorkOrder): Promise<{ success: boolean, printError?: string }>`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
