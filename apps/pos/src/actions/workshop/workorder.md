---
type: Reference
title: "apps/pos/src/actions/workshop/workorder.ts"
description: "Provides domain functionality for workorder."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.849854Z }
tags: ['service-workshop']
---

## Purpose & Responsibilities
- Provides domain functionality for workorder.

## Public Contract & Capabilities
- **Exposed Server Actions / Functions:**
  - `createWorkOrderAction(input: CreateWorkOrderInput): Promise<ActionResult<{ workorderId: string, printed: boolean, printError?: string }>>`
  - `checkAvailableSlotsAction(input: { date: string; durationMinutes: number; mechanicId?: string; }): Promise<ActionResult<string[]>>`
  - `startLaborTimerAction(input: { workorderId: string; mechanicId: string }): Promise<ActionResult<{ success: boolean, startTime: string }>>`
  - `stopLaborTimerAction(input: { workorderId: string; mechanicId: string; notes?: string }): Promise<ActionResult<{ success: boolean, endTime: string, durationMinutes: number }>>`
  - `updateWorkOrderStatusAction(input: { workorderId: string; status: string }): Promise<ActionResult<{ success: boolean }>>`
- **Key Invariants:** Location Isolation, Integer Cents for currency, ISO-8601 UTC dates.

## Current Features & Behaviors
- Implements specific business rules for this domain.

## Agreed-Upon Future Goals & Wishlist
- Improve observability and testing.
