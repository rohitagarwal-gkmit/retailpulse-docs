# Data Flow Diagrams

These diagrams illustrate the sequence of events for key processes in the system.

---

## 1. User Login Flow
*This flow is generic for any user role.*

```mermaid
sequenceDiagram
    actor User
    participant UI as Frontend
    participant API as Backend
    participant DB as Database

    User->>UI: Enters username and password
    UI->>API: POST /auth/login with credentials
    API->>DB: Find user by username and verify password
    DB-->>API: Return user data, including role and store_id
    alt Login successful
        API-->>UI: Return JWT token with user info
        UI-->>User: Redirect to role-specific dashboard
    else Login failed
        API-->>UI: Return 401 Unauthorized error
        UI-->>User: Show "Invalid credentials" message
    end
```

---

## 2. Bill Creation Flow
*Shows a Store Clerk making a sale.*

```mermaid
sequenceDiagram
    actor Store Clerk
    participant UI as Frontend
    participant API as Backend
    participant DB as Database
    
    Store Clerk->>UI: Searches for product and selects a batch
    UI->>API: GET /stores/{store_id}/inventory-items?product=...
    API-->>UI: Returns available batches for the product
    
    Store Clerk->>UI: Adds item to cart and clicks "Generate Bill"
    UI->>API: POST /stores/{store_id}/bills with bill data
    
    API->>DB: 1. Begin Transaction
    API->>DB: 2. Create new record in 'bills' table
    API->>DB: 3. Create new records in 'bill_items' table
    API->>DB: 4. Update quantity in 'inventory_items' for each batch sold
    API->>DB: 5. Commit Transaction
    
    API-->>UI: Return success with new bill ID
    UI-->>Store Clerk: Show "Bill created" message and offer PDF download
```

---

## 3. Add New Inventory Batch Flow
*Shows a Store Stockist adding a new shipment.*

```mermaid
sequenceDiagram
    actor Store Stockist
    participant UI as Frontend
    participant API as Backend
    participant DB as Database

    Store Stockist->>UI: Fills 'Receive Stock' form with batch details
    UI->>API: POST /stores/{store_id}/inventory-items with new item data
    API->>DB: Create new record in 'inventory_items' table
    DB-->>API: Return success
    API-->>UI: Return success
    UI-->>Store Stockist: Show "New inventory added" message
```

---

## 4. Internal Stock Movement Flow
*Shows a Store Stockist moving items from the storeroom to a shelf.*

```mermaid
sequenceDiagram
    actor Store Stockist
    participant UI as Frontend
    participant API as Backend
    participant DB as Database

    Store Stockist->>UI: Selects inventory item and clicks "Move Stock"
    UI->>API: POST /inventory-items/movements with movement details
    
    API->>DB: 1. Begin Transaction
    API->>DB: 2. Check if quantity is sufficient at source location
    API->>DB: 3. Update location and/or quantity of 'inventory_items' record(s)
    API->>DB: 4. Create a new record in 'inventory_movements' to log the action
    API->>DB: 5. Commit Transaction
    
    API-->>UI: Return success
    UI-->>Store Stockist: Show "Stock moved successfully" message
```

---

## 5. Inter-Store Stock Transfer Flow
*Shows a Company Stockist sending stock from Store A to Store B.*

```mermaid
sequenceDiagram
    actor Company Stockist
    participant UI as Frontend
    participant API as Backend
    participant DB as Database
    
    Company Stockist->>UI: Fills 'New Store Transfer' form
    UI->>API: POST /inventory-transfers with transfer details
    
    API->>DB: 1. Create 'inventory_transfers' record with 'PENDING' status
    API->>DB: 2. Reduce quantity from 'inventory_items' at sending store (or mark as 'IN_TRANSIT')
    API-->>UI: Return success
    
    Note over UI, DB: Later, at the receiving store...
    
    actor Store Stockist (Receiver)
    Store Stockist->>UI: Views pending transfers and clicks "Confirm Receipt"
    UI->>API: PATCH /inventory-transfers/{transfer_id} with 'COMPLETED' status
    
    API->>DB: 1. Update 'inventory_transfers' record to 'COMPLETED'
    API->>DB: 2. Add or update 'inventory_items' record at the receiving store
    API-->>UI: Return success
    UI-->>Store Stockist (Receiver): Show "Transfer complete" message
```

---

## 6. View Company-Wide Analytics Flow
*Shows a Company Admin viewing a sales report.*

```mermaid
sequenceDiagram
    actor Company Admin
    participant UI as Frontend
    participant API as Backend
    participant DB as Database

    Company Admin->>UI: Opens Analytics Dashboard
    UI->>API: GET /analytics/sales (no store_id filter)
    API->>DB: Query 'bills' and 'bill_items' tables across ALL stores
    DB-->>API: Return aggregated sales data
    API-->>UI: Send formatted data for charts and tables
    UI-->>Company Admin: Display company-wide sales report
```