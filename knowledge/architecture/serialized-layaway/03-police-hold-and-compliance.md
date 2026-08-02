# Police Hold and Compliance

## Configurable Store Location Toggle
Second-hand dealer legal compliance requirements vary significantly by jurisdiction. To accommodate this, the system uses an `enable_police_hold_compliance` toggle in the store configuration, allowing the feature to be enabled or disabled on a per-location basis.
- When `false`: Trade-in items skip police holds and move directly to refurbishment or sellable inventory.
- When `true`: The system activates the Second-Hand Dealer Legal Compliance Engine for the location.

## Second-Hand Dealer Legal Compliance Engine
When the `enable_police_hold_compliance` toggle is active, the following workflows and constraints are enforced during the trade-in process:

### Customer ID Capture
The system mandates the collection of customer identification data to comply with local laws. This includes:
- Driver's license scanning or manual entry
- State ID details
- Date of Birth
- Digital signature capture

### Hold Location Routing
Upon intake, the traded-in bicycle is automatically routed to a designated, non-sellable location in the inventory system (`TRADE_IN_HOLD`). This prevents the item from being inadvertently sold or modified while the hold is active.

### Hold Countdown State Machine
The system manages a configurable hold duration (e.g., 15 days or 30 days depending on local laws) using an automated state machine.
- The initial status is set to `POLICE_HOLD_ACTIVE`.
- An automated daily cron job monitors the elapsed time and automatically updates the status to `POLICE_HOLD_CLEARED` once the required duration has passed.

### Serial Report Export Engine
To simplify compliance reporting to local authorities, the system includes a 1-click export engine. This allows staff to quickly generate daily or weekly second-hand trade-in logs, properly formatted for submission to local law enforcement or LEADS registry systems.