# Data Flow Diagrams

## 1. Bill Creation Flow

This shows the steps for creating a bill.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor Manager
    participant UI as Frontend
    participant API as Backend
    participant DB as Database
    
    Manager->>UI: Fills out bill form
    UI->>API: Sends bill data
    API->>DB: Save bill details
    API->>DB: Update product stock
    API-->>UI: Return success
    UI-->>Manager: Show success message
```

---

## 2. User Login Flow

This shows how a user logs in to the system.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor User
    participant UI as Frontend
    participant API as Backend
    participant DB as Database

    User->>UI: Enters username and password
    UI->>API: Sends login credentials
    API->>DB: Check username and password
    DB-->>API: Return user data
    alt Login successful
        API-->>UI: Return success with user role
        UI-->>User: Redirect to dashboard
    else Login failed
        API-->>UI: Return error
        UI-->>User: Show error message
    end
```

---

## 3. Add New Product Flow

This shows how an Admin adds a new product.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor Admin
    participant UI as Frontend
    participant API as Backend
    participant DB as Database

    Admin->>UI: Fills product form
    UI->>API: Sends product data
    API->>DB: Save product
    DB-->>API: Return success
    API-->>UI: Return success
    UI-->>Admin: Show "Product Added" message
```

---

## 4. Update Product Stock Flow

This shows how an Admin updates product stock.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor Admin
    participant UI as Frontend
    participant API as Backend
    participant DB as Database

    Admin->>UI: Enters new stock quantity
    UI->>API: Sends product ID and new quantity
    API->>DB: Update stock in inventory table
    DB-->>API: Return success
    API-->>UI: Return success
    UI-->>Admin: Show "Stock Updated" message
```

---

## 5. View Reports Flow

This shows how an Admin views sales and inventory reports.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'18px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor Admin
    participant UI as Frontend
    participant API as Backend
    participant DB as Database

    Admin->>UI: Opens reports page
    UI->>API: Request reports data
    API->>DB: Query bills and inventory
    DB-->>API: Return data
    API-->>UI: Send formatted data
    UI-->>Admin: Display charts and tables
```