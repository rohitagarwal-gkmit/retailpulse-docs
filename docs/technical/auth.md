# Authentication and Authorization

## Authentication Flow

1.  A user enters their username and password on the login page.
2.  The system checks if the credentials are correct.
3.  If they are correct, the user is logged in and can access the app.
4.  If they are incorrect, an error message is shown.

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
