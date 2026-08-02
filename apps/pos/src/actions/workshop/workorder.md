---
type: Module
resource: apps/pos/src/actions/workshop/workorder.ts
domain: Service Workshop
status: Implemented
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
