# Functional Documentation

## User Roles and Permissions

To support a growing multi-store wholesale business, we have expanded our user roles. The system now supports a clear hierarchy, ensuring staff can only access the information they need.

| Role | Scope | Key Responsibilities |
|---|---|---|
| **Company Admin** | All Stores | Has complete control. Manages stores, oversees all inventory, views company-wide analytics, and manages all user accounts. |
| **Store Manager** | Assigned Store | Manages a single store. Oversees daily operations, manages staff, views store-level analytics, and handles local inventory. |
| **Clerk** | Assigned Store | Handles day-to-day sales. Primarily responsible for creating bills for customers (e.g., pharmacies, clinics). |
| **Stockist** | Assigned Store | Manages the inventory for a single store. Responsible for receiving new medicine shipments, organizing them, and tracking their location. |
| **Company Stockist** | All Stores | A central role for inventory oversight. Can view inventory across all stores and authorize/initiate stock transfers between them. |
| **Sales** | Assigned Stores | Views sales reports and product performance for one or more stores to analyze trends and performance. |

---

## System Modules Overview

RetailPulse is structured into five main modules, designed to support a multi-store medical wholesale operation.

---

## Module 1: Bill Generation

### Purpose
To enable fast and accurate billing for customers at the store level, with all data automatically synced.

### Features

#### 1.1 Medicine Search and Selection
- **Store-Specific Search**: Find medicines available at the user's current store.
- **Detailed View**: See stock availability for different batches (e.g., with different expiry dates) before adding to a bill.
- **Multi-Product**: Add multiple medicines to a single bill.

#### 1.2 Real-time Stock Validation
- **Batch-Level Check**: When a medicine is added to a bill, the system checks the quantity available in a specific batch.
- **Quantity Lock**: Ensures the amount requested does not exceed what's available in the selected batch.

#### 1.3 Bill Creation Form
- **Customer Info**: Add customer name (e.g., pharmacy name) and contact details.
- **Product List**: Shows medicine name, batch number, expiry date, unit price, quantity, and total price.
- **Calculations**: Automatically calculates subtotal, discount, tax, and the grand total.

#### 1.4 Bill Numbering and Tracking
- **Smart Numbering**: Bills get a unique number that includes the store ID (e.g., `STORE1-BILL-00001`).
- **Audit Trail**: Records the date, time, and the clerk who created the bill.

#### 1.5 PDF Generation
- **Customized PDFs**: Bills are generated in a professional PDF format with the specific store's logo and address.
- **Instant Download**: Download the PDF right after creating the bill.

#### 1.6 Automatic Inventory Update
- **Deduct from Batch**: When a bill is finalized, the stock quantity is automatically deducted from the correct batch in the inventory.
- **Sales Log**: Every sale is logged for analytics.

---

## Module 2: Inventory Management

### Purpose
To provide powerful, granular control over pharmaceutical inventory across all stores, from receiving shipments to tracking their final sale.

### Features

#### 2.1 Medicine Master List
- **Central Catalog**: A central list of all medicines the company sells, managed by Company Admins.
- **Basic Info**: Includes medicine name, category, manufacturer, and description.

#### 2.2 Detailed Inventory Tracking
- **Batch & Expiry**: Each medicine delivery is stored as a unique batch with its own batch number, manufacturing ID, expiry date, cost price, and selling price.
- **Location Management**: Track exactly where each batch is located within a store (e.g., `Shelf A1`, `Storeroom Rack 3`).
- **Storage Categories**: Assign a storage type to each item, such as `Cold Storage` or `General`, to ensure proper handling of sensitive medicines.

#### 2.3 View Inventory
- **Company-Wide View**: Company Admins and Company Stockists can see inventory levels across all stores.
- **Store-Level View**: Store Managers and Stockists can see a detailed view of the inventory in their own store.
- **Advanced Filtering**: Filter inventory by medicine, category, expiry date (e.g., "expiring in 30 days"), or location.

#### 2.4 Stock Adjustments and Movement
- **Manual Adjustments**: Manually change stock quantity with a mandatory reason (e.g., "Damaged stock," "Cycle count correction").
- **Internal Movement**: Log the movement of stock from one location to another within the same store (e.g., from the storeroom to a shelf).
- **Inter-Store Transfers**: A formal process for Company Stockists to initiate and track the transfer of stock from one store to another.

---

## Module 3: Analytics Dashboard

### Purpose
To provide actionable insights for different roles, from store-level performance to company-wide trends.

### Features

#### 3.1 Multi-Store Sales Analytics
- **Company View**: Admins can see total revenue, profit, and sales trends across all stores. They can also compare the performance of different stores.
- **Store View**: Store Managers can see detailed sales analytics for their own store.

#### 3.2 Advanced Product Performance
- **Best Sellers**: Identify top-selling medicines by store, region, or across the entire company.
- **Batch Profitability**: Analyze the profitability of different batches of the same medicine, which may have been purchased at different prices.

#### 3.3 Inventory Insights
- **Aging Inventory**: Generate reports to identify stock that is nearing its expiry date.
- **Stock Valuation**: View the total value of inventory, broken down by store and product category.
- **Movement History**: See a log of how stock has moved, helping to identify bottlenecks or optimize layout.

#### 3.4 Time Range Filters
- **Flexible Filtering**: Filter all reports by standard (today, this week) or custom date ranges.

---

## Module 4: Authentication and Authorization

### Purpose
To secure the system and ensure users only access what their role permits.

### Features

#### 4.1 User Registration and Management
- **Admin Control**: Only Company Admins can create or deactivate stores and other users.
- **Store Assignment**: When creating a user, an Admin assigns them a role and, if applicable, a home store.

#### 4.2 User Login
- **Secure Login**: Users log in with a username and password.
- **Session Management**: The system manages user sessions and provides automatic logouts for security.

#### 4.3 Role-Based Access Control (RBAC)
- **Granular Permissions**: The system enforces the permissions defined in the **User Roles** table. For example, a `Clerk` from Store A cannot create a bill for Store B, nor can they see Store B's inventory. A `Company Admin` can do both.

---

## Module 5: Role-Based Dashboards

### Purpose
To provide each user with an immediate, relevant overview of their tasks and key metrics as soon as they log in. Each dashboard is tailored to the user's specific role.

### Features

#### 5.1 Company Admin Dashboard
- **View**: Company-wide ("bird's-eye") view.
- **Key Metrics**: Total revenue, total profit, and total sales across all stores.
- **Visuals**: A map showing all store locations, charts comparing store performance.
- **Alerts**: Notifications for system-wide issues, low-performing stores, or large-scale stock shortages.
- **Quick Links**: Manage Stores, Manage Users, Company-Wide Analytics.

#### 5.2 Store Manager Dashboard
- **View**: Focused on their single, assigned store.
- **Key Metrics**: Total revenue, profit, and sales for their store.
- **Visuals**: Charts showing daily/weekly sales trends, top-selling medicines, and inventory value for their store.
- **Alerts**: Notifications for stock expiring soon, low stock levels, and pending inter-store transfers for their store.
- **Quick Links**: Create Bill, View Store Inventory, Store Analytics, Manage Staff (Clerks/Stockists).

#### 5.3 Clerk Dashboard
- **View**: A streamlined interface focused on the primary task of billing.
- **Default Page**: The "Create Bill" page is the main dashboard.
- **Visuals**: A simple list of their own recent bills created during the current session.
- **Alerts**: None by default, to maintain a clean interface.
- **Quick Links**: Create Bill, View Own Bill History.

#### 5.4 Stockist Dashboard
- **View**: Focused on inventory management for their assigned store.
- **Key Metrics**: Total inventory value, number of unique items, and stock-out percentage for their store.
- **Visuals**: A list of pending tasks.
- **Alerts**: Notifications for newly received shipments, pending stock counts, and items with low stock.
- **Quick Links**: View Store Inventory, Receive Stock, Move Stock, View Transfers.

#### 5.5 Company Stockist Dashboard
- **View**: Company-wide inventory overview.
- **Key Metrics**: Total inventory value across all stores, number of pending inter-store transfers.
- **Visuals**: A table showing stock levels of key medicines across all stores for easy comparison.
- **Alerts**: Notifications for new transfer requests and completed transfers.
- **Quick Links**: View All Inventory, Initiate Transfer, View Transfer History.

#### 5.6 Sales Dashboard
- **View**: Focused on sales performance for their assigned region or stores.
- **Key Metrics**: Sales revenue vs. target, number of bills created, and average bill value.
- **Visuals**: Leaderboards showing top-selling medicines and top-performing stores in their region.
- **Alerts**: Notifications for significant sales events or when a target is met.
- **Quick Links**: View Sales Reports, Product Performance Analytics.