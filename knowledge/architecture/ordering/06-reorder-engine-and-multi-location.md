# Reorder Engine and Multi-Location

## Dynamic Reorder Calculation

To prevent stockouts while minimizing holding costs, the module calculates suggested reorders dynamically rather than relying solely on static Min/Max thresholds.

### Core Variables & Formulas

1. **Average Daily Velocity (ADV):**
   The trailing 90-day sales volume divided by 90.
   $$ \text{ADV} = \frac{\text{Sales Last 90 Days}}{90} $$

2. **Safety Stock (SS):**
   Buffer inventory based on lead time variability and service level targets.
   $$ \text{SS} = (\text{Max Daily Sales} \times \text{Max Lead Time}) - (\text{ADV} \times \text{Average Lead Time}) $$

3. **Reorder Point (ROP):**
   The inventory level that triggers a reorder alert.
   $$ \text{ROP} = (\text{ADV} \times \text{Average Lead Time}) + \text{SS} $$

4. **Suggested Order Quantity (SOQ):**
   The exact amount to order to return inventory to optimal levels.
   $$ \text{SOQ} = \text{Target Maximum Inventory} - \text{Current On-Hand} - \text{On-Order Quantity} $$

## Seasonal Demand Index

The bicycle industry is highly seasonal. To prevent over-ordering in fall and stockouts in spring, the ADV is adjusted using a monthly multiplier.

*Example Northern Hemisphere Multipliers:*
- **January:** 0.45 (Low Demand)
- **February:** 0.60
- **March:** 1.10
- **April:** 1.50
- **May:** 1.65 (Peak Demand)
- **June:** 1.70 (Peak Demand)
- **July:** 1.40
- **August:** 1.20
- **September:** 1.00
- **October:** 0.75
- **November:** 0.50
- **December:** 0.65 (Holiday bump)

*Adjusted ADV = Base ADV × Current Month Index*

## Free Freight Aggregator

Distributors often require minimum order values (e.g., $500) to qualify for free shipping. The module includes a **Draft PO Progress Bar UI**:

- As special orders and ROP-triggered items are added to a Draft PO, a visual progress bar tracks the subtotal (in cents) against the vendor's `freeFreightThresholdCents`.
- **Smart Filler Suggestions:** If a PO is at $450 with a $500 threshold, the UI suggests highly liquid staple items (e.g., specific sizes of inner tubes, popular chain lubes, fast-moving brake pads) to intelligently bridge the $50 gap without accumulating dead stock.

## Multi-Location Transfers

For retail networks with multiple physical stores or an off-site warehouse:
- **Centralized Receiving:** All large vendor shipments arrive at the main hub.
- **Store Allocation:** The system parses the GRN and immediately suggests Transfer Orders (TOs) to push inventory to satellite stores based on their individual ROPs and pending special orders.
- **In-Transit Tracking Ledger:** When a TO is created and picked, the inventory leaves the Hub's `AVAILABLE` bucket and enters an `IN_TRANSIT` bucket. It only enters the Satellite store's `AVAILABLE` bucket once physically scanned and received at that specific location, ensuring total ledger accuracy across the enterprise.

## Developer Implementation Roadmap

The construction of this module is broken down into four distinct phases:

### Phase 1: Foundation (Weeks 1-3)
- Initialize Drizzle ORM schema (`schema/procurement.ts`).
- Build core CRUD Server Actions for Vendors and Vendor Mappings.
- Create basic Draft Purchase Order UI.

### Phase 2: Integrations (Weeks 4-7)
- Implement Unified Gateway and `DistributorAdapter` interface.
- Build QBP and Shimano REST adapters.
- Implement JIT search and concurrent API streaming.

### Phase 3: Hardware & Receiving (Weeks 8-10)
- Build touchscreen receiving UI loop.
- Implement barcode scanner hardware hooks.
- Develop the Landed Cost Allocation Engine and MAC reconciliation.

### Phase 4: Omnichannel & Automation (Weeks 11-14)
- Integrate Stripe Terminal for Special Order deposits.
- Implement Shopify GraphQL API sync (`fulfillment_hold`).
- Build Automated Notifications (Twilio SMS / Resend Email).
- Deploy Dynamic Reorder Engine with Seasonal Indexing.