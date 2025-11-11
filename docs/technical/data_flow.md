# System Data Flow

This document shows detailed data flow diagrams for each user role in the RetailPulse system.

---

## 1. Company Admin Data Flow

The Company Admin manages the entire system, including stores, users, and viewing all analytics.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
flowchart TD
    Admin[Company Admin] -->|Login credentials| P1((Authenticate))
    P1 --> Auth[Auth Module]
    Auth -->|Verify user| DB[(Database)]
    DB -->|User data| Auth
    Auth -->|Access granted| Admin
    
    Admin -->|Create store request| P2((Create Store))
    P2 --> Mgmt[Store & User Management]
    Mgmt -->|Save store data| DB
    DB -->|Confirmation| Mgmt
    Mgmt -->|Store created| Admin
    
    Admin -->|Create user request| P3((Create User))
    P3 --> Mgmt
    Mgmt -->|Save user data| DB
    DB -->|Confirmation| Mgmt
    Mgmt -->|User created| Admin
    
    Admin -->|Request analytics| P4((Generate Reports))
    P4 --> Anal[Analytics Module]
    Anal -->|Fetch data| DB
    DB -->|Sales & inventory data| Anal
    Anal -->|Formatted reports| Admin
```

**Key Functions:**
- Manage all stores (create, edit, delete)
- Manage all users across stores
- View company-wide analytics and reports
- Configure system settings

---

## 2. Store Manager Data Flow

The Store Manager oversees a specific store, manages store staff, and views store-level analytics.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
flowchart TD
    Manager[Store Manager] -->|Login credentials| P1((Authenticate))
    P1 --> Auth[Auth Module]
    Auth -->|Verify user| DB[(Database)]
    DB -->|User data with store| Auth
    Auth -->|Access granted| Manager
    
    Manager -->|Create staff request| P2((Create Staff))
    P2 --> Mgmt[User Management]
    Mgmt -->|Save user for store| DB
    DB -->|Confirmation| Mgmt
    Mgmt -->|Staff created| Manager
    
    Manager -->|View inventory| P3((View Stock))
    P3 --> Inv[Inventory Module]
    Inv -->|Query inventory| DB
    DB -->|Stock levels| Inv
    Inv -->|Inventory data| Manager
    
    Manager -->|Request reports| P4((Generate Reports))
    P4 --> Anal[Analytics Module]
    Anal -->|Fetch sales data| DB
    DB -->|Store analytics| Anal
    Anal -->|Formatted reports| Manager
```

**Key Functions:**
- Manage store staff (create Sales, Stockist users)
- View store inventory levels
- View store-specific sales analytics
- Monitor store performance

---

## 3. Sales Data Flow

The Sales user creates bills, searches for products, and handles customer transactions.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
flowchart TD
    Sales[Sales] -->|Login credentials| P1((Authenticate))
    P1 --> Auth[Auth Module]
    Auth -->|Verify user| DB[(Database)]
    DB -->|User data| Auth
    Auth -->|Access granted| Sales
    
    Sales -->|Search query| P2((Search Product))
    P2 --> Inv[Inventory Module]
    Inv -->|Query products| DB
    DB -->|Product list| Inv
    Inv -->|Available products| Sales
    
    Sales -->|Bill items| P3((Create Bill))
    P3 --> Bill[Billing Module]
    Bill -->|Save bill| DB
    Bill -->|Deduct stock request| Inv
    Inv -->|Update quantities| DB
    DB -->|Confirmation| Bill
    Bill -->|Bill & PDF| Sales
    
    Sales -->|Request reports| P4((View Sales))
    P4 --> Anal[Analytics Module]
    Anal -->|Query bills| DB
    DB -->|Sales data| Anal
    Anal -->|Formatted reports| Sales
```

**Key Functions:**
- Search for products/medicines
- Create customer bills
- Generate PDF bills
- View own sales reports
- Process customer payments

---

## 4. Stockist Data Flow

The Stockist manages local store inventory, receives stock, and updates quantities.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
flowchart TD
    Stockist[Stockist] -->|Login credentials| P1((Authenticate))
    P1 --> Auth[Auth Module]
    Auth -->|Verify user| DB[(Database)]
    DB -->|User data with store| Auth
    Auth -->|Access granted| Stockist
    
    Stockist -->|Request inventory| P2((View Inventory))
    P2 --> Inv[Inventory Module]
    Inv -->|Query store inventory| DB
    DB -->|Stock with batches| Inv
    Inv -->|Inventory details| Stockist
    
    Stockist -->|Stock details| P3((Receive Stock))
    P3 --> Inv
    Inv -->|Add stock & batch info| DB
    DB -->|Confirmation| Inv
    Inv -->|Stock updated| Stockist
    
    Stockist -->|Batch & expiry| P4((Update Batch))
    P4 --> Inv
    Inv -->|Update batch data| DB
    DB -->|Confirmation| Inv
    Inv -->|Updated| Stockist
```

**Key Functions:**
- View store inventory
- Receive new stock shipments
- Add products with batch and expiry details
- Update stock quantities
- Monitor stock levels and expiry dates

---

## 5. Company Stockist Data Flow

The Company Stockist oversees inventory across all stores and manages inter-store transfers.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
flowchart TD
    CompStockist[Company Stockist] -->|Login credentials| P1((Authenticate))
    P1 --> Auth[Auth Module]
    Auth -->|Verify user| DB[(Database)]
    DB -->|User data| Auth
    Auth -->|Access granted| CompStockist
    
    CompStockist -->|Request inventory| P2((View All Inventory))
    P2 --> Inv[Inventory Module]
    Inv -->|Query all stores| DB
    DB -->|All stores stock data| Inv
    Inv -->|Consolidated inventory| CompStockist
    
    CompStockist -->|Transfer details| P3((Transfer Stock))
    P3 --> Inv
    Inv -->|Deduct from source| DB
    Inv -->|Add to destination| DB
    Inv -->|Log transfer| DB
    DB -->|Confirmation| Inv
    Inv -->|Transfer complete| CompStockist
    
    CompStockist -->|Request alerts| P4((Check Expiry))
    P4 --> Inv
    Inv -->|Query near-expiry items| DB
    DB -->|Expiry data all stores| Inv
    Inv -->|Formatted alerts| CompStockist
```

**Key Functions:**
- View inventory across all stores
- Manage inter-store stock transfers
- Monitor company-wide stock levels
- Track expiry dates across all locations
- Optimize inventory distribution

---

## Summary

Each role has specific data flows tailored to their responsibilities:

| Role | Primary Functions | Key Modules Used |
|------|------------------|------------------|
| **Company Admin** | System management, company-wide analytics | Auth, Management, Analytics |
| **Store Manager** | Store staff management, store analytics | Auth, Management, Inventory, Analytics |
| **Sales** | Billing, product search, sales reports | Auth, Billing, Inventory, Analytics |
| **Stockist** | Local inventory management, stock receiving | Auth, Inventory |
| **Company Stockist** | Company-wide inventory, inter-store transfers | Auth, Inventory |