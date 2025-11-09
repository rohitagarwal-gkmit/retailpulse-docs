# Use Cases

## Primary Use Cases

### UC-01: Store Manager Creates a Bill

- **Actor**: Store Manager
- **Goal**: To create a bill for a customer quickly.
- **Flow**:
    1.  Manager goes to the "Create Bill" page.
    2.  Searches for a product and adds it to the bill.
    3.  Enters the quantity.
    4.  Repeats for all products.
    5.  Enters customer name and contact (optional).
    6.  Clicks "Generate Bill".
    7.  The system saves the bill, updates the stock, and creates a PDF.
    8.  Manager can download the PDF.
- **Alternative**: If the quantity is more than the stock, the system shows an error.

---

### UC-02: Admin Adds New Medicine to Inventory

- **Actor**: Admin
- **Goal**: To add a new product to the system.
- **Flow**:
    1.  Admin goes to "Inventory Management" and clicks "Add New Product".
    2.  Fills in product details: name, category, cost price, and selling price.
    3.  Clicks "Save Product".
    4.  The product is now in the inventory and can be sold.
- **Alternative**: If the product name already exists, the system shows an error.

---

### UC-03: Admin Views Sales Analytics

- **Actor**: Admin
- **Goal**: To see how the business is performing.
- **Flow**:
    1.  Admin goes to the "Analytics Dashboard".
    2.  The system shows key numbers like Total Revenue, Total Profit, and Bills Count.
    3.  The system shows charts for sales trends and top-selling products.
    4.  Admin can change the time range (e.g., last 30 days) to see different data.

---

### UC-04: Admin Updates Product Stock Manually

- **Actor**: Admin
- **Goal**: To manually change the stock quantity of a product.
- **Flow**:
    1.  Admin finds a product in the inventory and clicks "Edit Stock".
    2.  Enters the new quantity and a reason for the change (e.g., "New purchase").
    3.  Clicks "Save Adjustment".
    4.  The system updates the stock quantity and logs the change.
- **Alternative**: The system will not allow the stock to go below 0.

---

### UC-05: Manager Views Their Bill History

- **Actor**: Store Manager
- **Goal**: To see a list of bills they have created.
- **Flow**:
    1.  Manager goes to the "My Bills" page.
    2.  The system shows a list of their past bills, with date, customer name, and total.
    3.  Manager can click on a bill to see full details or download the PDF again.

---

### UC-06: Admin Manages User Accounts

- **Actor**: Admin
- **Goal**: To create or deactivate user accounts.
- **Flow (Create)**:
    1.  Admin goes to "User Management" and clicks "Add New User".
    2.  Enters the user's name, username, role (Admin or Manager), and a password.
    3.  Clicks "Create User". The new user can now log in.
- **Flow (Deactivate)**:
    1.  Admin finds a user and clicks "Deactivate".
    2.  The user's account is turned off and they can no longer log in.
- **Alternatives**:
    - Usernames must be unique.
    - An admin cannot deactivate their own account or the last remaining admin account.

---

### UC-07: User Logs In

- **Actor**: Admin or Store Manager
- **Goal**: To log into the system.
- **Flow**:
    1.  User goes to the login page.
    2.  Enters their username and password.
    3.  Clicks "Login".
    4.  The system logs them in and shows the correct dashboard for their role.
- **Alternatives**:
    - If the username or password is wrong, an error is shown.
    - If the account is deactivated, login fails.