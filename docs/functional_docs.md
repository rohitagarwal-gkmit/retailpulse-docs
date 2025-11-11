# Functional Documentation

## User Roles and Permissions

To support a growing multi-store wholesale business, we have expanded our user roles. The system now supports a clear hierarchy, ensuring staff can only access the information they need.

| Role | Scope | Key Responsibilities |
|---|---|---|
| **Company Admin** | All Stores | Has complete control. Manages stores, oversees all inventory, and manages all user accounts. |
| **Store Manager** | Assigned Store | Manages a single store. Oversees daily operations, manages staff, and handles local inventory. |
| **Stockist** | Assigned Store | Manages the inventory for a single store. Responsible for receiving new product shipments, organizing them, and tracking their location. |
| **Company Stockist** | All Stores | A central role for inventory oversight. Can view inventory across all stores and authorize/initiate stock transfers between them. |
| **Sales** | Assigned Store(s) | Creates bills for customers at the point of sale and handles customer interactions. |

---

## System Modules Overview

RetailPulse is structured into Three main modules, designed to support a multi-store medical wholesale operation.

---

## Module 1: Bill Generation

### Purpose
To enable fast and accurate billing for customers at the store level, with all data automatically synced, primarily handled by the Sales role.

### Features

#### 1.1 Product Search and Selection
- **Store-Specific Search**: Find products available at the user's current store.
- **Detailed View**: See stock availability for different batches (e.g., with different expiry dates) before adding to a bill.
- **Multi-Product**: Add multiple products to a single bill.

#### 1.2 Real-time Stock Validation
- **Batch-Level Check**: When a product is added to a bill, the system checks the quantity available in a specific batch.
- **Quantity Lock**: Ensures the amount requested does not exceed what's available in the selected batch.

#### 1.3 Bill Creation Form
- **Customer Info**: Add customer name (e.g., pharmacy name) and contact details.
- **Product List**: Shows product name, batch number, expiry date, unit price, quantity, and total price.
- **Calculations**: Automatically calculates subtotal, discount, tax, and the grand total.

#### 1.4 Bill Numbering and Tracking
- **Smart Numbering**: Bills get a unique number that includes the store ID (e.g., `STORE1-BILL-00001`).
- **Audit Trail**: Records the date, time, and the Sales user who created the bill.

#### 1.5 PDF Generation
- **Customized PDFs**: Bills are generated in a professional PDF format with the specific store's logo and address.
- **Instant Download**: Download the PDF right after creating the bill.

#### 1.6 Automatic Inventory Update
- **Deduct from Batch**: When a bill is finalized, the stock quantity is automatically deducted from the correct batch in the inventory.

---

## Module 2: Inventory Management

### Purpose
To provide powerful, granular control over product inventory across all stores, from receiving shipments to tracking their final sale.

### Features

#### 2.1 Product Master List
- **Central Catalog**: A central list of all products the company sells, managed by Company Admins.
- **Basic Info**: Includes product name, category, manufacturer, and description.

#### 2.2 Detailed Inventory Tracking
- **Batch & Expiry**: Each product delivery is stored as a unique batch with its own batch number, manufacturing ID, expiry date, cost price, and selling price.
- **Location Management**: Track exactly where each batch is located within a store (e.g., `Shelf A1`, `Storeroom Rack 3`).
- **Storage Categories**: Assign a storage type to each item, such as `Cold Storage` or `General`, to ensure proper handling of sensitive products.

#### 2.3 View Inventory
- **Company-Wide View**: Company Admins and Company Stockists can see inventory levels across all stores.
- **Store-Level View**: Store Managers and Stockists can see a detailed view of the inventory in their own store.
- **Advanced Filtering**: Filter inventory by product, category, expiry date (e.g., "expiring in 30 days"), or location.

#### 2.4 Stock Adjustments and Movement
- **Manual Adjustments**: Manually change stock quantity with a mandatory reason (e.g., "Damaged stock," "Cycle count correction").
- **Internal Movement**: Log the movement of stock from one location to another within the same store (e.g., from the storeroom to a shelf).
- **Inter-Store Transfers**: A formal process for Company Stockists to initiate and track the transfer of stock from one store to another.

---

## Module 3: Authentication and Authorization

### Purpose
To secure the system and ensure users only access what their role permits.

### Features

#### 3.1 User Registration and Management
- **Admin Control**: Only Company Admins can create or deactivate stores and other users.
- **Store Assignment**: When creating a user, an Admin assigns them a role and, if applicable, a home store.

#### 3.2 User Login
- **Secure Login**: Users log in with a username and password.
- **Session Management**: The system manages user sessions and provides automatic logouts for security.

#### 3.3 Role-Based Access Control (RBAC)
- **Granular Permissions**: The system enforces the permissions defined in the **User Roles** table. For example, a `Sales` user from Store A cannot create a bill for Store B, nor can they see Store B's inventory. A `Company Admin` can do both.