# Schema Design

## Database Overview

We use a PostgreSQL database to store all our data. It's reliable and keeps our information safe.

---

## Entity Relationship Diagram

This diagram shows how the different data tables are connected.

```mermaid
erDiagram
    USERS ||--o{ BILLS : creates
    PRODUCTS ||--o{ INVENTORY : has
    PRODUCTS ||--o{ BILL_ITEMS : contains
    BILLS ||--|{ BILL_ITEMS : has
    
    USERS {
        int id PK
        varchar username UK
        varchar password_hash
        varchar role
        varchar full_name
        boolean is_active
    }
    
    PRODUCTS {
        int id PK
        varchar name UK
        varchar category
        varchar manufacturer
        text description
        decimal cost_price
        decimal selling_price
        boolean is_active
    }
    
    INVENTORY {
        int id PK
        int product_id FK
        int quantity
    }
    
    BILLS {
        int id PK
        date bill_date
        varchar bill_number UK
        varchar customer_name
        varchar customer_contact
        decimal total_amount
        decimal discount
        decimal tax_amount
        decimal grand_total
        int created_by FK
        varchar pdf_path
    }
    
    BILL_ITEMS {
        int id PK
        int bill_id FK
        int product_id FK
        int quantity
        decimal unit_price
        decimal total_price
    }
```

---

## Table Schemas

### 1. users

This table holds information for user login.

```sql
CREATE TABLE users (
    id              SERIAL PRIMARY KEY,
    username        VARCHAR(50) UNIQUE NOT NULL,
    password_hash   VARCHAR(255) NOT NULL,
    role            VARCHAR(20) NOT NULL CHECK (role IN ('admin', 'manager')),
    full_name       VARCHAR(100),
    is_active       BOOLEAN DEFAULT TRUE NOT NULL
);
```

- `username`: The name a user types to log in.
- `password_hash`: The user's password, stored safely.
- `role`: What the user is allowed to do (admin or manager).

---

### 2. products

This table lists all the products we sell.

```sql
CREATE TABLE products (
    id              SERIAL PRIMARY KEY,
    name            VARCHAR(200) UNIQUE NOT NULL,
    category        VARCHAR(100),
    manufacturer    VARCHAR(100),
    description     TEXT,
    cost_price      DECIMAL(10,2) NOT NULL,
    selling_price   DECIMAL(10,2) NOT NULL,
    is_active       BOOLEAN DEFAULT TRUE NOT NULL
);
```

- `name`: The product's name.
- `cost_price`: How much we pay for the product.
- `selling_price`: How much we sell the product for.

---

### 3. inventory

This table tracks the stock level for each product.

```sql
CREATE TABLE inventory (
    id              SERIAL PRIMARY KEY,
    product_id      INTEGER NOT NULL REFERENCES products(id) ON DELETE RESTRICT,
    quantity        INTEGER NOT NULL DEFAULT 0
);
```

- `product_id`: Links to the product.
- `quantity`: How much stock is currently available.

---

### 4. bills

This table stores the main details of each customer bill.

```sql
CREATE TABLE bills (
    id                  SERIAL PRIMARY KEY,
    bill_number         VARCHAR(50) UNIQUE NOT NULL,
    customer_name       VARCHAR(100),
    customer_contact    VARCHAR(20),
    total_amount        DECIMAL(10,2) NOT NULL,
    discount            DECIMAL(10,2) DEFAULT 0,
    tax_amount          DECIMAL(10,2) DEFAULT 0,
    grand_total         DECIMAL(10,2) NOT NULL,
    created_by          INTEGER NOT NULL REFERENCES users(id),
    bill_date           DATE NOT NULL,
    pdf_path            VARCHAR(255)
);
```

- `bill_number`: A unique number for the bill.
- `customer_name`: The customer's name (optional).
- `grand_total`: The final amount the customer pays.
- `created_by`: The user who made this bill.
- `pdf_path`: Location where the PDF bill is stored.

---

### 5. bill_items

This table lists each product sold within a bill.

```sql
CREATE TABLE bill_items (
    id              SERIAL PRIMARY KEY,
    bill_id         INTEGER NOT NULL REFERENCES bills(id) ON DELETE CASCADE,
    product_id      INTEGER NOT NULL REFERENCES products(id),
    quantity        INTEGER NOT NULL,
    unit_price      DECIMAL(10,2) NOT NULL,
    total_price     DECIMAL(10,2) NOT NULL
);
```

- `bill_id`: Links to the bill this item is part of.
- `product_id`: Links to the product that was sold.
- `quantity`: How many units of the product were sold.
- `total_price`: Total price for this line item (quantity × unit_price).