---
type: Reference
title: "03_Security_And_Data_Mutation_Governance"
description: "Knowledge document for 03_Security_And_Data_Mutation_Governance."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.861815Z }
---

# 03. Security & Data Mutation Governance

## 1. Unified Action Result Wrapper (`ActionResult<T>`)

### Rationale & Design Invariant
Uncaught exceptions thrown across Server Actions trigger Next.js generic error boundaries, crashing the POS interface. All Server Actions must return a standardized, type-safe result object.

### Rules
All Server Actions MUST return the unified `ActionResult<T>` type:

```typescript
export type ActionResult<T> =
  | { success: true; data: T }
  | { success: false; error: string; code?: string };
2. Zero-Trust Input Sanitization & SQLi/XSS Defense
Rationale & Design Invariant
The POS platform processes untrusted input from web forms, barcode scanners, customer records, external webhooks, and hardware interfaces.

Rules
Zod Boundary Enforcement: All Server Action parameters, REST inputs, and webhook payloads MUST pass through Zod schema validation at the runtime boundary.

SQL Injection Prevention: Direct string concatenation in SQL queries is strictly prohibited. All queries must use Drizzle ORM query builders or parameterized sql template literals.

XSS Prevention: User-supplied text rendered in React DOM or receipt templates MUST NOT use dangerouslySetInnerHTML unless wrapped in an audited sanitization utility (e.g., DOMPurify).

3. AI Prompt Injection Mitigation & LLM Guardrails
Rationale & Design Invariant
AI agents integrated into the POS platform must be immune to direct prompt injections (user inputs trying to alter system prompts) and indirect prompt injections (untrusted vendor catalog text or customer notes executing malicious LLM instructions).

Rules
Strict Role Separation: System instructions MUST NEVER be concatenated directly with runtime user input or database text.

Structural Delimiter Wrapping: Untrusted user input, web search results, or vendor descriptions passed to an LLM context window MUST be enclosed inside explicit structural tags:

Plaintext
<untrusted_user_input>
${sanitizedInput}
</untrusted_user_input>
Structured Output Enforcement: AI outputs MUST NEVER be executed as raw code or direct SQL. AI outputs triggering system mutations MUST return JSON that passes strict Zod schema validation.

Least Privilege Execution: AI tool calls MUST execute through the exact same authorization, location checks, and business rule boundaries as human cashiers.