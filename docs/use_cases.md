# Use Cases

## Primary Use Cases

### UC-01: Sales Creates a Bill

- **Actor**: Sales
- **Goal**: To create a bill for a customer (e.g., a pharmacy) at their assigned store.
- **Flow**:
    1.  Sales logs in and is on the "Create Bill" page for their store.
    2.  Searches for a product. The system shows available batches with expiry dates and quantities.
    3.  Selects a batch and adds it to the bill.
    4.  Enters the quantity.
    5.  Repeats for all products.
    6.  Enters customer name and contact (optional).
    7.  Clicks "Generate Bill".
    8.  The system saves the bill, deducts the quantity from the specific inventory batch, and generates a PDF with the store's details.
- **Alternative**: If the requested quantity is more than the stock in the selected batch, the system shows an error.

---

### UC-02: Stockist Adds New Inventory

- **Actor**: Stockist
- **Goal**: To add a new shipment of products into the store's inventory.
- **Flow**:
    1.  Stockist goes to "Inventory Management" and clicks "Receive Stock".
    2.  Selects the product from the master list.
    3.  Enters the new stock details: batch number, manufacturing ID, expiry date, quantity received, cost price, and selling price.
    4.  Assigns a location (e.g., "Storeroom Rack 5") and storage category (e.g., "Cold Storage").
    5.  Clicks "Save".
    6.  The new batch is now part of the store's inventory and is available for sale.
- **Alternative**: If the product is not on the master list, the Stockist must ask a Company Admin to add it first.

---

### UC-03: Company Admin Views Company-Wide Analytics

- **Actor**: Company Admin
- **Goal**: To understand and compare the performance of all stores.
- **Flow**:
    1.  Admin goes to the "Analytics Dashboard".
    2.  The dashboard defaults to a company-wide view, showing total revenue, profit, and top-selling products across all stores.
    3.  Admin uses a filter to select "Store A" and "Store B" to see a side-by-side comparison of their sales.
    4.  Admin then navigates to the "Inventory" analytics tab to see the total stock value per store.

---

### UC-04: Stockist Moves Stock Internally

- **Actor**: Stockist
- **Goal**: To move a product from the storeroom to a picking shelf.
- **Flow**:
    1.  Stockist finds the inventory item (e.g., Paracetamol, Batch #123) in the system.
    2.  Clicks "Move Stock".
    3.  Enters the quantity to move (e.g., 20 units).
    4.  Sets the "From Location" to "Storeroom Rack 5" and "To Location" to "Picking Shelf C1".
    5.  Adds a reason: "Restocking picking shelf".
    6.  Clicks "Confirm Movement".
    7.  The system updates the location of the 20 units and logs the movement for auditing.

---

### UC-05: Company Stockist Transfers Stock Between Stores

- **Actor**: Company Stockist
- **Goal**: To move surplus stock from one store to another that needs it.
- **Flow**:
    1.  Company Stockist identifies that "Store A" has 200 units of a product, while "Store B" has only 5.
    2.  They initiate a "New Store Transfer".
    3.  They select the product, the "From Store" (Store A), and the "To Store" (Store B).
    4.  They enter the quantity to transfer (e.g., 100 units).
    5.  The system creates a transfer order. The 100 units at Store A are marked as "In Transit".
    6.  When the stock arrives at Store B, the Stockist there confirms receipt.
    7.  The system deducts the stock from Store A's inventory and adds it to Store B's inventory.

---

### UC-06: Company Admin Manages Users and Stores

- **Actor**: Company Admin
- **Goal**: To manage the company's structure, including stores and staff.
- **Flow (Add Store)**:
    1.  Admin goes to "Store Management" and clicks "Add New Store".
    2.  Enters the store name, address, and contact info. Clicks "Save".
- **Flow (Add User)**:
    1.  Admin goes to "User Management" and clicks "Add New User".
    2.  Enters the user's name, username, and password.
    3.  Assigns a role (e.g., "Store Manager") and a home store ("Store B").
    4.  Clicks "Create User". The user can now log in and will only have access to Store B.

---

### UC-07: Store Manager Checks for Expiring Products

- **Actor**: Store Manager
- **Goal**: To identify products that are nearing their expiry date to prioritize their sale.
- **Flow**:
    1.  Manager goes to the "Inventory Reports" section for their store.
    2.  Runs the "Expiring Soon" report, with a filter for "Next 60 days".
    3.  The system generates a list of all product batches that will expire in the next 60 days, showing the product name, batch number, quantity, and expiry date.
    4.  The manager can use this list to create a sales promotion or instruct staff to sell these batches first.