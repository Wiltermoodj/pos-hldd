---
type: Reference
title: "04 Automated Lifecycle Marketing Triggers"
description: "Knowledge document for 04 Automated Lifecycle Marketing Triggers."
status: stable
generated: { by: reference_agent/jules, at: 2026-08-02T20:45:15.854338Z }
---

# Automated Lifecycle Marketing Triggers

## Kids' Bike Size Growth Reminders
The system proactively tracks the purchase dates of children's bikes (e.g., 12", 16", 20", 24" wheel sizes) associated with a household's `bike_asset` records.
- **Automated Outreach:** After a configured interval (typically 18 months from the date of purchase), the system triggers an automated SMS or Email to the primary account holder.
- **Example Messaging:** *"Maya might be outgrowing her 20" Riprock! Bring it in for a trade-in evaluation toward a 24" mountain bike."*

This workflow drives repeat business and trade-in inventory while providing highly personalized, relevant customer service.

## Service & Maintenance Interval Triggers
To ensure optimal performance of customer assets and drive service department revenue, automated triggers manage ongoing maintenance reminders based on purchase or last-service dates.
- **6-Month Milestones:** Automated reminders sent 6 months post-sale or post-tuneup, prompting customers for a chain stretch inspection and suspension air/oil service.
- **Seasonal Preparation:** Automated annual tune-up reminders dispatched prior to the spring riding season (e.g., February/March), ensuring customer bikes are ready for peak usage.

## Outbound Dispatch Pipeline
The delivery of lifecycle marketing messages is handled by a robust outbound dispatch pipeline.
- **Integrations:** Utilizes the Twilio API for SMS communications and the Resend API for email delivery.
- **Compliance & Tracking:** The pipeline includes built-in unsubscribe management (handling STOP replies and unsubscribe links) and maintains detailed engagement activity logs (sent, delivered, opened, clicked) for performance analysis and compliance auditing.
