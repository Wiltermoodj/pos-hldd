---
type: Reference
title: "02 Hardware Integration And Listeners"
description: "Knowledge document for 02 Hardware Integration And Listeners."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.860029Z }
---

# Hardware Integration & Listeners

## Global Barcode Scanner Hook (`useBarcodeScanner`)
To provide a seamless scanning experience without the need for manual UI focus management, a global barcode scanner hook is implemented.
- **Document-Level Keydown Events**: Listens to keystrokes at the document level, capturing input regardless of which UI element is currently focused.
- **Inter-Character Timing Analysis**: Leverages the fact that hardware scanners emulate fast keyboard typing. It analyzes the time between keystrokes; scanners typically fire characters sequentially <20ms apart.
- **Isolating Scanner Strings**: By analyzing timing and waiting for the terminating Enter/Return key event, the hook automatically isolates scanner strings from manual keyboard typing. This ensures rapid scanning works reliably without dropping inputs if cursor focus drops or shifts.

## Protocol-Agnostic Receipt Printing Architecture
The receipt printing system is designed with a decoupled architecture, allowing flexibility in how and where receipts are printed.
- **`ReceiptFormatter` Interface**: A decoupled interface that produces standardized receipt layout Data Transfer Objects (DTOs). These DTOs encapsulate logical receipt sections: Header, Line Items, Taxes, Payment Summary, Barcode, and Return Policy.
- **Pluggable Transport Adapters**: The transport layer is abstracted. This allows the system to use various adapters to send the formatted receipt data to the physical printer. Supported transports include:
  - Direct browser ESC/POS via WebUSB or WebSerial APIs for compatible hardware.
  - Local proxy services (e.g., a lightweight Node.js or Rust service running on the local network/machine) that relay the commands to standard network or USB printers.

## Cash Drawer Pulse Driver
- **Cash Drawer Kick Pulse**: A dedicated driver is responsible for triggering the cash drawer. It sends a specific pulse signal (typically a DC24V pin 2 signal over an RJ11 connection connected to the receipt printer) to physically pop the cash drawer open.
- **Trigger Conditions**: The driver is automatically triggered upon the successful completion of transactions involving cash, or when dispensing cash change, as well as during authorized "No Sale" or "Paid Out" operations.
