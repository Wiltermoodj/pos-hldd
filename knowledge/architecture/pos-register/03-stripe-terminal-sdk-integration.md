---
type: Concept
title: "Stripe Terminal SDK Integration"
---

# Stripe Terminal SDK Integration

## Card-Present Terminal Integration
The POS system tightly integrates with Stripe Terminal to process in-person, card-present transactions.
- **Stripe Terminal SDK**: Utilizes the official SDK to establish connections to smart readers, primarily targeting models like the Stripe Reader S700 or BBPOS WisePOS E.
- **LAN/Wi-Fi Connectivity**: The connection to the terminal is established over the local network (LAN/Wi-Fi).
- **Connection Management**: Robust handling of connection states, including reader discovery on the network, seamless connection, and automatic reconnect handlers in case of temporary network drops or terminal restarts.

## Transaction Flow
- **Payment Intents**: Follows the standard Stripe payment intent flow adapted for terminal usage. The POS application creates a PaymentIntent on the backend and pushes it to the connected terminal.
- **Payment Collection**: The terminal prompts the customer to present their payment method. Supported methods include:
  - EMV Chip inserts.
  - Contactless / NFC payments (Apple Pay, Google Pay, tap-to-pay cards).
  - Magnetic swipe (fallback).
- **State Handling**: The system manages all states of the transaction flow, including handling payment cancellation by the customer or operator, and gracefully managing payment failures (e.g., declined cards, network errors).

## Workshop Labor Tip Prompting
- **Configurable Terminal Prompts**: For service repair transactions, the system can configure the terminal screen to present a tip prompt directly to the customer.
- **Customizable Options**: The prompt is highly customizable, presenting options such as: *"Add a tip for the workshop mechanic? 15% / 18% / 20% / Custom / No Tip"*. This allows customers to easily add gratuity for labor services directly on the smart reader.
