# Database Schema

## Overview

The system uses **PostgreSQL** as the database. The schema is designed to support a multi-store medical retail business, with detailed inventory tracking and a flexible Role-Based Access Control (RBAC) system.

To ensure data integrity and history, all tables include `created_at`, `updated_at`, and `deleted_at` timestamps for soft deletes.

---

## Entity Relationship Diagram

```mermaid
erDiagram
    users ||--|{ user_roles : "has"
    roles ||--o{ user_roles : "has many"
    users ||--|{ user_stores : "is in"
    stores ||--o{ user_stores : "has many"
    roles ||--|{ role_permissions : "has many"
    permissions ||--o{ role_permissions : "has many"
    stores ||--|{ bills : "generates"
    stores ||--|{ inventory_items : "has"
    users ||--o{ bills : "creates"
    users ||--o{ inventory_movements : "performs"
    products ||--o{ inventory_items : "is an instance of"
    bills ||--|{ bill_products : "contains"
    products ||--o{ bill_products : "appears in"
    inventory_items ||--o{ inventory_movements : "is moved in"

    users {
        int id PK
        varchar username UK
        varchar password_hash
        varchar full_name
        boolean is_active
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    roles {
        int id PK
        varchar name UK
        text description
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }
    
    user_roles {
        int id PK
        int user_id FK
        int role_id FK
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    permissions {
        int id PK
        varchar name UK
        text description
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    role_permissions {
        int id PK
        int role_id FK
        int permission_id FK
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    stores {
        int id PK
        varchar name UK
        text address
        varchar contact_phone
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    user_stores {
        int id PK
        int user_id FK
        int store_id FK
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    products {
        int id PK
        varchar name UK
        varchar category
        varchar manufacturer
        text description
        boolean is_active
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    inventory_items {
        int id PK
        int product_id FK
        int store_id FK
        varchar batch_number
        varchar manufacturing_id
        date expire_date
        int quantity
        varchar location
        varchar storage_category
        decimal cost_price
        decimal selling_price
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    inventory_movements {
        int id PK
        int inventory_item_id FK
        int moved_by_user_id FK
        int quantity_moved
        varchar from_location
        varchar to_location
        text reason
        timestamptz movement_date
        timestamptz created_at
        timestamptz updated_at
        timestamptz deleted_at
    }

    bills {
        int id PK
        int store_id FK
        int created_by_user_id FK
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
        int id PK
        int bill_id FK
        int product_id FK
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
The `stores` table contains a record for each retail store location. It includes a unique `id`, the store's `name`, `address`, and `contact_phone`. Timestamps for creation, updates, and soft deletes are included.

### `users`
The `users` table is the central record for every person who can log in to the system. It contains a unique `id`, `username`, `password_hash`, `full_name`, and an `is_active` flag. A user's roles and store assignments are managed in separate junction tables, so this table does not contain `role_id` or `store_id`. It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `roles`
The `roles` table defines the user roles available in the system. It has a unique `id` and a `name` for the role (e.g., 'Company Admin', 'Store Manager', 'Sales'), along with a `description`. It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `user_roles`
This is a junction table that assigns roles to users, creating a many-to-many relationship. It links `user_id` to `role_id`, allowing a single user to have multiple roles (e.g., a user could be both a 'Store Manager' and 'Sales'). It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `permissions`
The `permissions` table defines granular permissions for actions within the system. It has a unique `id` and a `name` for the permission (e.g., 'bills.create', 'analytics.view.all'), with a `description` of what it allows. It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `role_permissions`
This is a junction table that assigns permissions to roles, creating a many-to-many relationship. It links `role_id` to `permission_id`, ensuring each permission is assigned to a role only once. It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `user_stores`
This is a junction table that assigns users to stores, creating a many-to-many relationship. It links `user_id` to `store_id`, allowing a single user to be associated with multiple stores. It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `products`
The `products` table is the master catalog of all products the company sells. It contains the product's `id`, `name`, `category`, `manufacturer`, `description`, and an `is_active` flag. It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `inventory_items`
This is the core inventory table, tracking specific batches of products in specific stores and locations. It links to `products` and `stores` and includes a `batch_number`, `manufacturing_id`, `expire_date`, `quantity`, `location`, `storage_category`, `cost_price`, and `selling_price`. It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `inventory_movements`
The `inventory_movements` table logs the movement of inventory items from one location to another, creating an audit trail. It records the `inventory_item_id`, the `moved_by_user_id`, `quantity_moved`, `from_location`, `to_location`, a `reason`, and the `movement_date`. It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `bills`
The `bills` table stores header information for each bill (receipt). It is linked to the `stores` where it was created and the `users` who created it. It also contains a unique `bill_number`, customer details, and financial totals (`total_amount`, `discount`, `tax_amount`, `grand_total`). It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.

### `bill_products`
The `bill_products` table stores the individual line items for each bill. It links to the `bills` table and the specific `products` sold. It includes the `quantity`, `unit_price`, `total_price`, and the `batch_number` of the item sold for accurate inventory tracking. It also includes `created_at`, `updated_at`, and `deleted_at` timestamps.