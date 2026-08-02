---
type: Module
resource: apps/pos/src/lib/printer/escpos-encoder.ts
domain: POS Register
status: Implemented
---


## Purpose & Responsibilities
- Provides domain functionality for escpos-encoder.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `class EscPosEncoder`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
