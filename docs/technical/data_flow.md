# Data Flow Diagrams

## 1. Bill Creation Flow

This shows the steps for creating a bill.

```mermaid
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

-   **Key Point**: The bill creation and stock update happen in a single, safe transaction. If one part fails, everything is undone.
