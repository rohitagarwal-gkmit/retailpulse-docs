# Authentication and Authorization

## Authentication Flow

A user enters their username and password on the login page. The system checks if the credentials are correct. If they are, the user is logged in and can access the app. If not, an error message is shown.

```mermaid
graph TD
    A[User enters credentials on login page] --> B{Are credentials valid?};
    B -- Yes --> C[Log in user];
    B -- No --> G[Show error message];
    C --> D{Check user role};
    D -- Admin --> E[Redirect to Admin Dashboard];
    D -- Manager --> F[Redirect to Manager Dashboard];
```

## Authorization Rules

The system uses Role-Based Access Control (RBAC) to decide who can do what.

| Action | Manager | Admin |
|---|---|---|
| Create Bills | ✅ | ✅ |
| View Own Bills | ✅ | ✅ |
| View All Bills | ❌ | ✅ |
| View Analytics | ❌ | ✅ |
| Add/Edit Products | ❌ | ✅ |
| Manage Users | ❌ | ✅ |
