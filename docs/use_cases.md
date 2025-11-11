# Use Cases

## Core System Use Cases

### UC-01: Sales User Processes a Customer Order

-   **Actor**: Sales
-   **Goal**: To efficiently create a bill for a customer, ensuring accurate product selection and inventory deduction.
-   **Flow**:
    1.  The Sales user logs into the system and navigates to the "Create Bill" interface for their assigned store.
    2.  They search for a product by name or code. The system displays available batches of the product, including expiry dates and current quantities in stock.
    3.  The Sales user selects the desired product batch and specifies the quantity. The system validates that the requested quantity does not exceed the available stock in that batch.
    4.  They can add multiple products to the same bill, repeating the search and selection process.
    5.  Optionally, the Sales user enters customer details (e.g., name, contact information).
    6.  Upon confirming the order, the Sales user clicks "Generate Bill".
    7.  The system records the bill, automatically deducts the sold quantity from the specific inventory batch, and generates a printable PDF of the bill, customized with the store's details.
-   **Alternative**: If the requested quantity for a product exceeds the available stock in the selected batch, the system displays an error message, prompting the user to adjust the quantity or select a different batch.

---

### UC-02: Stock Management and Inter-Store Transfers

-   **Actors**: Stockist, Company Stockist
-   **Goal**: To maintain accurate inventory levels within stores and facilitate efficient product movement between locations.
-   **Flow (Stockist - Receiving Inventory)**:
    1.  A Stockist logs in and accesses the "Receive Stock" function within their assigned store's inventory management module.
    2.  They select a product from the master catalog and enter details for the new shipment, including batch number, manufacturing ID, expiry date, quantity received, cost price, and selling price.
    3.  The Stockist assigns a specific physical `location` (e.g., "Storeroom Rack 5") and `storage_category` (e.g., "Cold Storage") for the received batch.
    4.  Upon saving, the new batch is added to the store's inventory and becomes available for sale or internal movement.
-   **Flow (Company Stockist - Transferring Inventory Between Stores)**:
    1.  A Company Stockist identifies a need to transfer products between stores (e.g., Store A has surplus, Store B has a shortage).
    2.  They initiate a "New Store Transfer," selecting the product, the "From Store" (e.g., Store A), and the "To Store" (e.g., Store B), along with the quantity to transfer.
    3.  The system marks the transferred quantity as "In Transit" in the "From Store's" inventory.
    4.  Once the products arrive at the "To Store," a Stockist at that store confirms receipt in the system.
    5.  The system then deducts the stock from the "From Store's" inventory and adds it to the "To Store's" inventory, updating locations as necessary.

---

### UC-03: Company Admin Manages System Resources

-   **Actor**: Company Admin
-   **Goal**: To oversee and configure the entire system, including managing stores, users, and the master product catalog.
-   **Flow (Managing Stores)**:
    1.  The Company Admin logs in and navigates to the "Store Management" section.
    2.  They can add new stores by providing details such as name, address, and contact information.
    3.  Existing store details can be updated, or stores can be deactivated as needed.
-   **Flow (Managing Users)**:
    1.  The Company Admin accesses the "User Management" section.
    2.  They can create new user accounts, providing a username, password, and full name.
    3.  Crucially, the Admin assigns one or more `roles` to the new user (e.g., 'Store Manager', 'Sales') and associates them with one or more `stores` via the `user_roles` and `user_stores` junction tables.
    4.  Existing user accounts can be updated (e.g., changing roles, store assignments) or deactivated.
-   **Flow (Managing Product Master Catalog)**:
    1.  The Company Admin goes to the "Product Master List" section.
    2.  They can add new products to the central catalog, specifying details like name, category, manufacturer, and description.
    3.  Existing product details can be updated, or products can be marked as inactive if they are no longer sold.