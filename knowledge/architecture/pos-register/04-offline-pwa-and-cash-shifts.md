# Offline PWA & Cash Shifts

## Offline Transaction Queue (IndexedDB + ServiceWorker)
To ensure high availability and resilience against internet outages, the POS acts as a Progressive Web App (PWA) with robust offline capabilities.
- **Local PWA Caching**: Utilizing ServiceWorkers, the application caches essential static assets. More importantly, it caches fast-moving product data and active customer balances in local IndexedDB storage, allowing critical checkout operations to continue without a network connection.
- **Offline Processing**: If the network connection drops during checkout, the sale is seamlessly processed locally. It is assigned a specialized `OFFLINE_` prefixed receipt number and queued within the local IndexedDB.
- **Background Sync**: An automatic background sync worker monitors network status. Upon network restoration, it systematically pushes the queued offline transactions from IndexedDB to the centralized Neon PostgreSQL database, ensuring eventual consistency.

## Cash Shift Management Engine
The system includes a comprehensive cash management engine to track physical currency flow accurately.
- **Opening Shift**: The process begins with the declaration of a starting cash float (e.g., $200.00). This sets the baseline for the register's cash drawer.
- **Mid-Shift Operations**:
  - **Cash Drops**: Provides a workflow for moving excess cash from the register drawer to a secure safe during a shift, reducing risk.
  - **Paid-In/Paid-Out Logs**: Allows operators to log miscellaneous cash additions or removals (e.g., using register cash to pay a window cleaner), keeping the expected drawer total accurate.
- **Blind Closing Count**: At the end of a shift, the register clerk is required to enter the physical count of bills and coins in the drawer. This is a "blind" count, meaning the system does not reveal the *expected* total, significantly deterring theft and ensuring accurate reporting.
- **Reconciliation & Z-Report**: The system calculates the cash variance using the formula: `Over/Short = Actual Cash - (Opening Float + Cash Sales - Cash Drops)`. It then generates a comprehensive Z-Report summary receipt detailing the shift's financial performance and cash integrity.
