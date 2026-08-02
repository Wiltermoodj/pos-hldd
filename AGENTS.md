---
type: Reference
title: "Agent Instructions"
description: "Instructions and rules for AI Agents working in this repository."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T21:02:53.209059Z }
---

# Instructions for AI Agents Working in This Repository

## Core Mission
Maintain, expand, and structure high-level documentation and feature tracking specs for the POS platform using Open Knowledge Format (OKF). See the [OKF Spec](./OKF_SPEC.md) for details.

---

## ABSOLUTE STRICT INVARIANTS — DO NOT VIOLATE

1. **NO SOURCE CODE:**
   * NEVER paste, commit, or leak raw JavaScript, TypeScript, SQL, or function bodies.
   * Document ONLY interfaces, types, parameters, data intent, and operational rules.

---

## OKF Markdown Standard Template

When creating or updating any mirrored file spec, adhere strictly to this template:

---
type: Reference
title: "[File/Module Path]"
description: "[Brief high-level description]"
status: [draft | stable | deprecated]
tags: [[Domain Category]]
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
