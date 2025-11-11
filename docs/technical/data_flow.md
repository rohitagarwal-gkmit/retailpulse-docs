# System Data Flow

This document shows detailed data flow diagrams for each user role in the RetailPulse system.

---

## 1. Company Admin Data Flow

The Company Admin manages the entire system, including stores, users, and viewing all analytics.

```mermaid
flowchart TD
    Admin[Company Admin] -->|Store details| P1((Manage Stores))
    P1 --> Mgmt[Store & User Management]
    Mgmt -->|Read/Write| DB[(Database)]
    
    Admin -->|User details| P2((Manage Users))
    P2 --> Mgmt
    
    Admin -->|Request reports| P3((View Analytics))
    P3 --> Anal[Analytics Module]
    Anal -->|Read| DB
    Anal -->|Reports| Admin
```

**Key Functions:**
- Manage all stores (create, edit, delete)
- Manage all users across stores
- View company-wide analytics and reports

---

## 2. Store Manager Data Flow

The Store Manager oversees a specific store, manages store staff, and views store-level analytics.

```mermaid
flowchart TD
    Manager[Store Manager] -->|Staff details| P1((Manage Staff))
    P1 --> Mgmt[User Management]
    Mgmt -->|Write| DB[(Database)]
    
    Manager -->|Request inventory| P2((View Inventory))
    P2 --> Inv[Inventory Module]
    Inv -->|Read| DB
    Inv -->|Stock data| Manager
    
    Manager -->|Request reports| P3((View Reports))
    P3 --> Anal[Analytics Module]
    Anal -->|Read| DB
    Anal -->|Store analytics| Manager
```

**Key Functions:**
- Manage store staff (create Sales, Stockist users)
- View store inventory levels
- View store-specific sales analytics

---

## 3. Sales Data Flow

The Sales user creates bills, searches for products, and handles customer transactions.

```mermaid
flowchart TD
    Sales[Sales] -->|Search query| P1((Search Product))
    P1 --> Inv[Inventory Module]
    Inv -->|Read| DB[(Database)]
    Inv -->|Product list| Sales
    
    Sales -->|Bill items| P2((Create Bill))
    P2 --> Bill[Billing Module]
    Bill -->|Save bill| DB
    Bill -->|Deduct stock| Inv
    Inv -->|Update stock| DB
    Bill -->|Bill & PDF| Sales
```

**Key Functions:**
- Search for products
- Create customer bills and generate PDF
- Automatic inventory deduction

---

## 4. Stockist Data Flow

The Stockist manages local store inventory, receives stock, and updates quantities.

```mermaid
flowchart TD
    Stockist[Stockist] -->|Request| P1((View Inventory))
    P1 --> Inv[Inventory Module]
    Inv -->|Read| DB[(Database)]
    Inv -->|Stock with batches| Stockist
    
    Stockist -->|Stock & batch details| P2((Receive Stock))
    P2 --> Inv
    Inv -->|Add stock| DB
    Inv -->|Confirmation| Stockist
```

**Key Functions:**
- View store inventory with batch details
- Receive and add new stock with expiry dates

---

## 5. Company Stockist Data Flow

The Company Stockist oversees inventory across all stores and manages inter-store transfers.

```mermaid
flowchart TD
    CompStockist[Company Stockist] -->|Request| P1((View All Inventory))
    P1 --> Inv[Inventory Module]
    Inv -->|Read all stores| DB[(Database)]
    Inv -->|Consolidated data| CompStockist
    
    CompStockist -->|Transfer details| P2((Transfer Stock))
    P2 --> Inv
    Inv -->|Update stores| DB
    Inv -->|Confirmation| CompStockist
    
    CompStockist -->|Request| P3((Check Expiry))
    P3 --> Inv
    Inv -->|Read| DB
    Inv -->|Expiry alerts| CompStockist
```

**Key Functions:**
- View inventory across all stores
- Manage inter-store stock transfers
- Monitor expiry dates company-wide
