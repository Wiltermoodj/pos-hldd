# Instructions for AI Agents Working in This Repository

## Core Mission
Maintain, expand, and structure high-level documentation and feature tracking specs for the POS platform using Open Knowledge Format (OKF).

---

## ABSOLUTE STRICT INVARIANTS — DO NOT VIOLATE

1. **NO SOURCE CODE:**
   * NEVER paste, commit, or leak raw JavaScript, TypeScript, SQL, or function bodies.
   * Document ONLY interfaces, types, parameters, data intent, and operational rules.
2. **OKF COMPLIANCE:**
   * All markdown files must follow the Open Knowledge Format outlined in `knowledge/okf-spec.md`.
   * All non-reserved markdown files must start with a valid YAML frontmatter block containing a `type` field.

---

## OKF Markdown Standard Template

When creating or updating any mirrored file spec, adhere strictly to this OKF-compliant template:

```markdown
---
type: Module
resource: [File/Module Path]
domain: [e.g., POS Register, Workshop, Inventory Ledger, Procurement]
status: [Implemented / Hardened / Planned]
---

## Purpose & Responsibilities
- [Brief high-level description of what this module or view achieves]

## Public Contract & Capabilities
- **Exposed Actions / Hooks:** [List action names, inputs, and return interfaces]
- **Key Invariants:** [List domain rules: Integer Cents, Location Isolation, etc.]

## Current Capabilities
- [Bullet points of current functional features]

## Planned Goals & Community Wishlist
- [Bullet points of future enhancements and open feature requests]
```
