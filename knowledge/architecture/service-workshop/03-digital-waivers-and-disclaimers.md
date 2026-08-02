# Digital Waivers & Disclaimers

## Feature Toggle Architecture
Digital liability waivers are not uniformly required across all jurisdictions or business models.
- **Configuration:** The digital waiver functionality is controlled by the `enable_digital_waiver` store configuration setting, allowing it to be toggled per store location.

## Mandatory Retailer Disclaimer
When digital waivers are enabled, the software enforces strict communication regarding legal responsibility.
- **Admin UI Banner:** A prominent banner and documentation are displayed in the admin panel explicitly stating:

> *"LEGAL DISCLAIMER: The software provider supplies the digital signature capture technology only. Retailers are strictly required to consult their own legal counsel to draft and supply their own custom liability waiver text. The retailer is solely responsible for ensuring that all waiver text, terms, and digital signature capture procedures comply with applicable local, state, federal, and provincial laws."*

## Touchscreen Signature Capture Engine
The physical capture of customer consent is designed for modern POS hardware.
- **Canvas-based Capture:** A robust canvas-based signature capture engine is presented on the POS register or the customer-facing display during the intake workflow.

## Waiver Payload Storage
To ensure enforceability and auditability, comprehensive data regarding the signing event is securely stored in PostgreSQL (`digital_waivers`).
- **Captured Data Includes:**
  - Captured signature SVG vector paths.
  - Precise signing timestamp.
  - Customer IP address and device metadata.
  - The exact, verbatim signed text version payload to ensure the terms accepted at the time of signing cannot be retroactively altered.
