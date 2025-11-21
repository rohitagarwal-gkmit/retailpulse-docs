# Database Schema

## Overview

The system uses **PostgreSQL** for its ACID compliance, strong relational support, and efficient handling of complex joins needed for multi-store inventory and RBAC.

Users can have multiple roles and work across multiple stores via junction tables (`user_stores` and `user_roles`). All tables use soft deletes with `created_at`, `updated_at`, and `deleted_at` timestamps.

## Why PostgreSQL

- **ACID compliance**: Financial and inventory transactions require guaranteed consistency
- **Complex relationships**: Multiple many-to-many relationships (users-roles, users-stores, bills-products)
- **Structured data**: Product details, batches, pricing, and transactions are inherently structured
- **SQL querying**: Efficient aggregations and reporting on relational data
- **RBAC support**: Natural fit for role-permission modeling

---

## Entity Relationship Diagram

```mermaid
erDiagram
    users ||--|{ user_roles : "has"
    roles ||--o{ user_roles : "has many"
    users ||--|{ user_stores : "is in"
    stores ||--o{ user_stores : "has many"

    stores ||--|{ bills : "generates"
    stores ||--|{ inventory_items : "has"
    users ||--o{ bills : "creates"

    categories ||--o{ products : "has"
    products ||--o{ inventory_items : "is an instance of"
    bills ||--|{ bill_products : "contains"
    products ||--o{ bill_products : "appears in"


    users {
        uuid id PK
        varchar username UK
        varchar password_hash
        varchar full_name
        boolean is_active
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    roles {
        uuid id PK
        varchar name UK
        text description
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    user_roles {
        uuid id PK
        uuid user_id FK
        uuid role_id FK
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }



    stores {
        uuid id PK
        varchar name UK
        text address
        varchar contact_phone
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    user_stores {
        uuid id PK
        uuid user_id FK
        uuid store_id FK
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    categories {
        uuid id PK
        varchar name UK
        text description
        boolean is_active
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    products {
        uuid id PK
        uuid category_id FK
        varchar name UK
        varchar manufacturer
        text description
        decimal price
        boolean is_active
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    inventory_items {
        uuid id PK
        uuid product_id FK
        uuid store_id FK
        varchar batch_number
        date expire_date
        int quantity
        varchar location
        decimal cost_price
        decimal selling_price
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }



    bills {
        uuid id PK
        uuid store_id FK
        uuid created_by_user_id FK
        varchar bill_number UK
        varchar customer_name
        varchar customer_contact
        decimal total_amount
        decimal discount
        decimal tax_amount
        decimal grand_total
        timestamptz bill_date
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    bill_products {
        uuid id PK
        uuid bill_id FK
        uuid product_id FK
        varchar batch_number
        int quantity
        decimal unit_price
        decimal total_price
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }
```

---

## Database Tables

### `stores`

Store locations with name, address, and contact info.

### `users`

Central user records with username, password hash, and full name. Roles and stores assigned via junction tables.

### `roles`

Defines available roles (Company Admin, Store Manager, Sales, Stockist, Company Stockist).

### `user_roles`

Junction table linking users to roles (many-to-many).

### `user_stores`

Junction table linking users to stores (many-to-many).

### `categories`

Product categories with name and description for organizing products.

### `products`

Master product catalog with category reference, name, manufacturer, and description.

### `inventory_items`

Batch-level inventory tracking per store with expiry dates, quantities, location, and pricing.

### `bills`

Bill headers with store, user, bill number, customer details, and totals.

### `bill_products`

Bill line items with product, quantity, pricing, and batch number for inventory tracking.

## Indexing Strategy

**Auto-created indexes:**

- Primary keys (all tables)
- Unique constraints (`username`, `bill_number`, product/role/permission `name`)

**Explicit B-tree indexes on:**

**Foreign keys** (for JOIN performance):

- `user_roles`: `user_id`, `role_id`
- `user_stores`: `user_id`, `store_id`
- `products`: `category_id`
- `inventory_items`: `product_id`, `store_id`
- `bills`: `store_id`, `created_by_user_id`
- `bill_products`: `bill_id`, `product_id`

**Query-heavy columns:**

- `inventory_items.expire_date` - frequent expiry checks and range queries
- `inventory_items.batch_number` - batch-specific searches
- `categories.name` - filtering by category
- `products.manufacturer` - filtering by manufacturer
- `bills.bill_date` - date-based reporting

**Why B-tree:**
Handles equality, range queries, sorting, and pattern matching efficiently - covers all common query patterns in the system.
