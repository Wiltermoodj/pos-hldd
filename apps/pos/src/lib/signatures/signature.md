---
type: Module
resource: apps/pos/src/lib/signatures/signature.ts
domain: POS Register
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for signature.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `processSignatureBase64(base64String: string): Buffer`
  - `linkSignatureToWorkOrder(base64String: string, workOrder: WorkOrderType): WorkOrderType`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
