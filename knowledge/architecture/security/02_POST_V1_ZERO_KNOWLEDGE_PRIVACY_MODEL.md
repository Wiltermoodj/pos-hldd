---
type: Reference
title: "02_Post_V1_Zero_Knowledge_Privacy_Model"
description: "Knowledge document for 02_Post_V1_Zero_Knowledge_Privacy_Model."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.853510Z }
---

[STATUS: DEFERRED TO POST-V1 PHASE 2 - DO NOT IMPLEMENT DURING V1 CODE BASE REMEDIATION]

# Zero-Knowledge Privacy Model

## Rationale for Deferred Implementation
Client-side encryption (CSE) and blind indexing add significant complexity to local DX, automated testing, and database debugging. Initial V1 development will use standard server-side field-level encryption and Zod sanitization.

## Post-V1 Architecture Target

- **Signal-Style Client-Side Encryption**: (WebCrypto AES-256-GCM) for customer PII, bike serial numbers, and notes before leaving local devices.
- **Master Organization Key (MOK)**: Derived locally with zero developer or cloud server key access.
- **Blind Indexing**: (`HMAC-SHA256`) for exact-match database queries without exposing plaintext fields.
