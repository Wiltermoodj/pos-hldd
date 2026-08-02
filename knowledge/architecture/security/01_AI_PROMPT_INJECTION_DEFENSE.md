---
type: Reference
title: "01_Ai_Prompt_Injection_Defense"
description: "Knowledge document for 01_Ai_Prompt_Injection_Defense."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.853275Z }
---

# AI Prompt Injection Mitigation & LLM Guardrails Architecture

## 1. Domain Objective
To protect the application from direct prompt injections (user trying to bypass system instructions via UI input) and indirect prompt injections (untrusted text from vendor catalogs, customer notes, or webhooks triggering malicious LLM behavior).

## 2. Structural Prompt Isolation
- **Role Isolation:** System instructions must remain completely distinct from runtime context. Never concatenate raw user input directly into system prompt strings.
- **Delimiter Wrapping:** Enclose all untrusted runtime data (user queries, product records, search results) inside explicit structural delimiters in the prompt context:

```
<untrusted_user_input>
${sanitizedInput}
</untrusted_user_input>
```

## 3. Data Ingestion & Context Sanitization
- Before inserting third-party text (e.g., customer reviews, product descriptions, email content) into an LLM context window:
- Strip system-like instructions (e.g., "Ignore previous instructions", "System:", "Developer Mode:").
- Escape control syntax and XML-like tags matching system prompt delimiters.

## 4. Structured Output Enforcement & Validation
- LLM outputs MUST NOT be treated as trusted executable code or raw SQL.
- All AI responses triggering system actions MUST be formatted as JSON and strictly validated against a Zod schema before executing any business logic or DB reads/writes.

## 5. Principle of Least Privilege for Agentic Tools
- AI agents and function-calling routines MUST pass through the exact same authorization and business rule validation layers as human cashiers.
- AI tools CANNOT bypass Server Action parameter checks or location permission boundaries.
