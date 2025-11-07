# Use Cases

## **Use Cases**

### UC1: Operator Uploads Sales Data

*   **Description**: The operator uploads daily POS sales reports (CSV or PDF) to the system.
*   **Actor**: Operator
*   **Preconditions**: Operator is logged in.
*   **Postconditions**: Sales data is processed, stored, and available for analytics. File upload status is tracked.

### UC2: Operator Uploads Supplier Bills

*   **Description**: The operator uploads supplier bills (CSV or PDF) to the system.
*   **Actor**: Operator
*   **Preconditions**: Operator is logged in.
*   **Postconditions**: Inventory is updated, cost prices are adjusted, and supplier expenses are recorded. File upload status is tracked.

### UC3: Admin Views Sales Analytics

*   **Description**: The admin views various sales-related analytics, including daily, weekly, monthly sales, top-selling products, and revenue trends.
*   **Actor**: Admin
*   **Preconditions**: Admin is logged in. Sales data has been processed.
*   **Postconditions**: Admin gains insights into sales performance.

### UC4: Admin Views Inventory Summary

*   **Description**: The admin views the current inventory levels and identifies low-stock items.
*   **Actor**: Admin
*   **Preconditions**: Admin is logged in. Supplier bills have been processed.
*   **Postconditions**: Admin can make informed decisions about restocking.

### UC5: Admin Views Profit and Expense Tracking

*   **Description**: The admin views overall profit margins and tracks supplier expenses.
*   **Actor**: Admin
*   **Preconditions**: Admin is logged in. Sales data and supplier bills have been processed.
*   **Postconditions**: Admin understands the financial health of the store.

### UC6: Admin Manages Inventory

*   **Description**: The admin can manually add, view, edit, and delete products in the inventory.
*   **Actor**: Admin
*   **Preconditions**: Admin is logged in.
*   **Postconditions**: Inventory is updated.

### UC7: User Logs In

*   **Description**: An admin or operator logs into the system to access their respective dashboards and functionalities.
*   **Actor**: Admin, Operator
*   **Preconditions**: User has valid credentials.
*   **Postconditions**: User is authenticated and a session (e.g., JWT) is created. User is redirected to their dashboard.