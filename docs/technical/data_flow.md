# Data Flow Diagrams

## 1. Bill Creation Flow

This shows the steps for creating a bill.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'38px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor Manager
    participant UI as React UI
    participant API as Backend
    participant DB as Database
    
    Manager->>UI: 1. Fills out the bill form
    UI->>API: 2. Sends bill data
    API->>DB: 3. BEGIN TRANSACTION
    API->>DB: 4. Save bill details
    API->>DB: 5. Update product stock
    API->>DB: 6. COMMIT TRANSACTION
    API-->>UI: 7. Confirms bill was created
    UI-->>Manager: 8. Shows success message
```

## 2. User Login Flow

This diagram shows how a user logs in and gets a session token.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'38px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor User
    participant UI as React UI
    participant API as Backend
    participant DB as Database

    User->>UI: 1. Enters username and password
    UI->>API: 2. Sends credentials
    API->>DB: 3. Queries for user with matching credentials
    DB-->>API: 4. Returns user data (with role)
    alt Credentials are valid
        API->>API: 5. Generates session token (JWT)
        API-->>UI: 6. Returns token to UI
        UI->>User: 7. Stores token and redirects to dashboard
    else Credentials are invalid
        API-->>UI: 8. Returns authentication error
        UI-->>User: 9. Shows error message
    end
```

## 3. Product Creation Flow

This flow describes how an Admin adds a new product to the inventory.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'38px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor Admin
    participant UI as React UI
    participant API as Backend
    participant DB as Database

    Admin->>UI: 1. Fills out product creation form
    UI->>API: 2. Sends new product data
    API->>API: 3. Validates product data (e.g., check for required fields)
    alt Data is valid
        API->>DB: 4. Inserts new product into 'Products' table
        DB-->>API: 5. Confirms insertion
        API-->>UI: 6. Returns success message
        UI-->>Admin: 7. Shows "Product Created" notification
    else Data is invalid
        API-->>UI: 8. Returns validation error
        UI-->>Admin: 9. Displays error message on the form
    end
```

## 4. Inventory Update Flow

This diagram shows how an Admin manually adjusts the stock for a product.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'38px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor Admin
    participant UI as React UI
    participant API as Backend
    participant DB as Database

    Admin->>UI: 1. Selects product and enters new stock quantity
    UI->>API: 2. Sends product ID, new quantity, and reason for change
    API->>DB: 3. BEGIN TRANSACTION
    API->>DB: 4. Updates stock quantity for the product
    API->>DB: 5. Logs the change in an 'InventoryAdjustments' table
    API->>DB: 6. COMMIT TRANSACTION
    API-->>UI: 7. Confirms stock was updated
    UI-->>Admin: 8. Shows success message
```

## 5. Analytics Dashboard Data Flow

This shows how the analytics dashboard gets its data.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'38px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor Admin
    participant UI as React UI
    participant API as Backend
    participant DB as Database

    Admin->>UI: 1. Navigates to Analytics Dashboard
    UI->>API: 2. Requests analytics data (with time filters)
    API->>DB: 3. Runs aggregation queries (e.g., SUM, COUNT, GROUP BY) on Bills and Products tables
    DB-->>API: 4. Returns aggregated sales, profit, and inventory data
    API->>API: 5. Formats data for charts (e.g., JSON for Chart.js)
    API-->>UI: 6. Sends formatted data to UI
    UI->>UI: 7. Renders charts and key metrics
    UI-->>Admin: 8. Displays dashboard
```

## 6. Product Details Update Flow

This diagram shows how an Admin updates the core details of a product, such as its price or description.

```mermaid
%%{init: {'theme':'base', 'themeVariables': { 'fontSize':'38px', 'fontFamily':'Arial'}}}%%
sequenceDiagram
    actor Admin
    participant UI as React UI
    participant API as Backend
    participant DB as Database

    Admin->>UI: 1. Selects a product and chooses to edit it
    UI->>UI: 2. Loads product details into a form
    Admin->>UI: 3. Modifies product details (e.g., price, name, category)
    UI->>API: 4. Sends updated product data
    API->>API: 5. Validates incoming data
    alt Data is valid
        API->>DB: 6. Updates the product's record in the 'Products' table
        DB-->>API: 7. Confirms the update
        API-->>UI: 8. Returns success message
        UI-->>Admin: 9. Shows "Product Updated" notification
    else Data is invalid
        API-->>UI: 10. Returns validation error
        UI-->>Admin: 11. Displays error message on the form
    end
```