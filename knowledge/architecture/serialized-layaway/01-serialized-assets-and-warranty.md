# Serialized Assets and Warranty

## Hierarchical Serial Asset Model
The system employs a hierarchical serial asset model to track various components of a bicycle. The primary identifier is the Frame Serial Number, which acts as the root for secondary component serials. These secondary serials can include:
- E-bike Battery ID
- Motor Serial
- Suspension Fork ID
- Power Meter ID

All these serial numbers are attached to a single `bike_asset` record, allowing for comprehensive tracking and management of the complete bicycle assembly.

## Stolen Bike Registry Integration
To prevent the intake or servicing of stolen bicycles, the system integrates with prominent stolen bike registries such as BikeIndex.org and Project 529. Pre-transaction API checks are performed automatically during:
- Trade-in intake
- Service check-in

These checks use the frame serial number to flag any bicycles reported as stolen, alerting the staff and halting the transaction if necessary.

## Automated Manufacturer Warranty Registration
To streamline post-sale processes and enhance customer experience, the system features automated manufacturer warranty registration. Post-sale webhook and API triggers are configured to submit necessary information to major brand portals (including Trek, Specialized, Cannondale, and Santa Cruz). The submitted data typically includes:
- Frame serials
- Customer details
- Invoice data

This ensures warranties are registered promptly and accurately without manual data entry.

## Asset Lifecycle States
A `bike_asset` record transitions through several defined states throughout its lifecycle within the system:
1. `IN_STOCK`: The asset is currently in inventory and available for sale.
2. `SOLD`: The asset has been purchased by a customer.
3. `IN_SERVICE`: The asset is currently in the workshop for maintenance or repair.
4. `TRADED_IN`: The asset has been accepted as a trade-in and is being evaluated or refurbished.
5. `ARCHIVED`: The asset is no longer actively tracked (e.g., destroyed, lost, or permanently transferred out of the system).