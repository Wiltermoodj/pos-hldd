---
# Module: apps/pos/src/lib/utils/zod-error.ts
**Domain Category:** POS Register
**Status:** Implemented

## Purpose & Responsibilities
- Provides domain functionality for zod-error.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `formatZodError(error: ZodError): string`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
---
