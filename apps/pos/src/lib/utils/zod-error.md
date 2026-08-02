---
type: Reference
title: "apps/pos/src/lib/utils/zod-error.ts"
description: "Provides domain functionality for zod-error."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.850933Z }
tags: ['pos-register']
---

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
