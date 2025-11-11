# System Data Flow

This document shows data flow sequence diagrams for each user role in the RetailPulse system.

---

## 1. Company Admin Data Flow

The Company Admin manages the entire system, including stores and users.

```mermaid
sequenceDiagram
    actor Admin as Company Admin
    participant System
    participant DB as Database
    
    Admin->>System: Manage Stores
    System->>DB: Create/Update/Delete Store
    DB-->>System: Confirmation
    System-->>Admin: Store Updated
    
    Admin->>System: Manage Users
    System->>DB: Create/Update/Delete User
    DB-->>System: Confirmation
    System-->>Admin: User Updated
```

**Key Functions:**
- Manage all stores (create, edit, delete)
- Manage all users across stores

---

## 2. Store Manager Data Flow

The Store Manager oversees a specific store, manages store staff, and views store-level inventory.

```mermaid
sequenceDiagram
    actor Manager as Store Manager
    participant System
    participant DB as Database
    
    Manager->>System: Manage Staff
    System->>DB: Create/Update Staff
    DB-->>System: Confirmation
    System-->>Manager: Staff Updated
    
    Manager->>System: View Inventory
    System->>DB: Get Store Inventory
    DB-->>System: Inventory Data
    System-->>Manager: Display Stock Levels
```

**Key Functions:**
- Manage store staff (create Sales, Stockist users)
- View store inventory levels

---

## 3. Sales Data Flow

The Sales user creates bills, searches for products, and handles customer transactions.

```mermaid
sequenceDiagram
    actor Sales
    participant System
    participant DB as Database
    
    Sales->>System: Search Product
    System->>DB: Query Products
    DB-->>System: Product List
    System-->>Sales: Display Products
    
    Sales->>System: Create Bill
    System->>DB: Save Bill
    System->>DB: Deduct Inventory
    DB-->>System: Confirmation
    System-->>Sales: Bill & PDF Generated
```

**Key Functions:**
- Search for products
- Create customer bills and generate PDF
- Automatic inventory deduction

---

## 4. Stockist Data Flow

The Stockist manages local store inventory, receives stock, and updates quantities.

```mermaid
sequenceDiagram
    actor Stockist
    participant System
    participant DB as Database
    
    Stockist->>System: View Inventory
    System->>DB: Get Store Stock
    DB-->>System: Stock with Batches
    System-->>Stockist: Display Inventory
    
    Stockist->>System: Receive Stock
    System->>DB: Add Stock & Batch Details
    DB-->>System: Confirmation
    System-->>Stockist: Stock Added Successfully
```

**Key Functions:**
- View store inventory with batch details
- Receive and add new stock with expiry dates

---

## 5. Company Stockist Data Flow

The Company Stockist oversees inventory across all stores and manages inter-store transfers.

```mermaid
sequenceDiagram
    actor CompStockist as Company Stockist
    participant System
    participant DB as Database
    
    CompStockist->>System: View All Inventory
    System->>DB: Get All Stores Inventory
    DB-->>System: Consolidated Data
    System-->>CompStockist: Display All Stock
    
    CompStockist->>System: Transfer Stock
    System->>DB: Update Source Store
    System->>DB: Update Destination Store
    DB-->>System: Confirmation
    System-->>CompStockist: Transfer Completed
```