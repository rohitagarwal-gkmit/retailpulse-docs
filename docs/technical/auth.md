# Authentication and Authorization

## Authentication Flow

A user enters their username and password on the login page. The system checks if the credentials are correct. If they are, the system identifies their role and redirects them to the appropriate dashboard. If not, an error message is shown.

```mermaid
graph TD
    A[User enters credentials on login page] --> B{Are credentials valid?};
    B -- Yes --> C[Log in user];
    B -- No --> G[Show error message];
    C --> D{Check user role};
    D --> E[Redirect to Role-Specific Dashboard];
```

## Authorization Rules

The system uses a granular Role-Based Access Control (RBAC) model to enforce permissions. A user's role determines what they can see and do, and their assigned store (if any) limits the scope of their actions.

Below is a summary of the permissions for each role.

| Action | Company Admin | Store Manager | Stockist | Company Stockist | Sales |
|---|:---:|:---:|:---:|:---:|:---:|
| **Store Management** | | | | | |
| Create/Edit Stores | ✅ | ❌ | ❌ | ❌ | ❌ |
| **User Management** | | | | | |
| Create/Edit Users | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Medicine Master List** | | | | | |
| Create/Edit Medicines | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Inventory** | | | | | |
| View Own Store Inventory | ✅ | ✅ | ✅ | ✅ | ✅ |
| View All Stores' Inventory | ✅ | ❌ | ✅ | ✅ | ✅ |
| Add/Edit Own Store Inventory | ✅ | ✅ | ✅ | ❌ | ❌ |
| Move Stock (within store) | ✅ | ✅ | ✅ | ❌ | ❌ |
| Transfer Stock (between stores) | ✅ | ❌ | ❌ | ✅ | ❌ |
| **Billing** | | | | | |
| Create/Edit Bills (own store) | ✅ | ✅ | ❌ | ❌ | ✅ |
| View Bills (own store) | ✅ | ✅ | ❌ | ❌ | ✅ |
| View Bills (all stores) | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Analytics** | | | | | |
| View Own Store Analytics | ✅ | ✅ | ❌ | ❌ | ✅ |
| View All Stores' Analytics | ✅ | ❌ | ❌ | ❌ | ❌ |
