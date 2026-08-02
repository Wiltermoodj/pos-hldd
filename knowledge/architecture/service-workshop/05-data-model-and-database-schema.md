# Data Model & Database Schema

The Service Workshop module utilizes a serverless database layer using Drizzle ORM configured for PostgreSQL (Neon compatibility). The production schema is defined in `schema/service_workshop.ts`.

## Production Drizzle ORM Schema (`schema/service_workshop.ts`)

### `bike_assets`
Stores unique details about every bike serviced, serving as the core entity for service history.
- `id`: Primary key
- `serial_number`: Unique identifier (often indexed)
- `brand`, `model`, `color`: Text descriptions
- `e_bike_details`: JSONB storing motor IDs, battery IDs, etc.
- `customer_id`: Link to the customer record

### `work_orders`
The central entity for a specific repair job.
- `id`: Primary key
- `work_order_number`: Unique, human-readable identifier
- `bike_asset_id`: Foreign key to `bike_assets`
- `customer_id`: Foreign key to customer record
- `assigned_mechanic_id`: Foreign key to `mechanic_schedules` or user record
- `promised_date`: Timestamp for expected completion
- `rack_location_tag`: String for physical storage location
- `status`: Enum (e.g., `INTAKE`, `IN_PROGRESS`, `COMPLETED`)
- `total_labor_cents`, `total_parts_cents`, `deposit_paid_cents`: Numeric financial fields

### `work_order_services`
Individual labor tasks required for the work order.
- `id`: Primary key
- `work_order_id`: Foreign key to `work_orders`
- `service_item_id`: Reference to the master service catalog
- `estimated_minutes`: Planned duration
- `actual_minutes`: Logged duration for mechanic time tracking
- `mechanic_id`: ID of the mechanic performing the service
- `status`: Tracking individual task completion

### `work_order_items`
Physical parts attached to or required for the work order.
- `id`: Primary key
- `work_order_id`: Foreign key to `work_orders`
- `product_id` / `variant_id`: Reference to inventory
- `quantity`: Number of items
- `unit_price_cents`, `unit_cost_cents`: Financial tracking
- `status`: Enum (`RECOMMENDED`, `APPROVED`, `INSTALLED`) for estimate flow

### `digital_waivers`
Records of signed liability waivers.
- `id`: Primary key
- `customer_id`: Foreign key to customer
- `work_order_id`: Foreign key to `work_orders`
- `waiver_text_version`: The verbatim text agreed to
- `signature_svg_vector`: Stored SVG paths of the signature
- `signed_timestamp`: Exact timestamp of signing
- `ip_address`, `device_metadata`: Audit trail data

### `mechanic_schedules`
Manages mechanic capacity for load balancing.
- `id`: Primary key
- `mechanic_id`: Foreign key to the user/mechanic
- `date`: Schedule date
- `available_labor_minutes`: Total available capacity for the day
- `booked_labor_minutes`: Current allocated capacity from assigned work orders

## Indexes & Relations

To ensure performance, especially for real-time workshop views, specific database features are heavily utilized:
- **Foreign Key Constraints:** Enforced across `work_orders`, `bike_assets`, and `customer_id`s to ensure data integrity.
- **Unique Indices:** `work_order_number` and `bike_assets.serial_number` (often scoped by brand) have unique indices to prevent duplication.
- **Fast Lookup Indexes:** Compound indexes are applied on `work_orders.status` and `work_orders.promised_date` to accelerate queries populating active workshop queues and Kanban boards.
