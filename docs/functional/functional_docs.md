# Functional Documentation

## **User Roles**

| Role                   | Access  | Capabilities                                       |
| ---------------------- | ------- | -------------------------------------------------- |
| **Admin**              | Full    | View analytics, profit, inventory trends           |
| **Operator**           | Limited | Upload sales data (PDF/CSV), upload supplier bills |

---

## **Modules Overview**

### 1. Sales Upload (POS Reports)

* Operator uploads `.csv` or `.pdf`
* Stored in S3 bucket
* Tracking table logs file details: `filename`, `upload_time`, `uploaded_by`, `isProcessed`
* ETL job (Pandas) extracts and standardizes data
* Data loaded to `sales` and `products` tables

### 2. Supplier Bills Upload

* Operator uploads supplier bill (CSV or PDF)
* Extracted using PDF parser
* Updates `inventory` table (add stock, update cost price)
* Option for Admin to review uploads in dashboard

### 3. Analytics Dashboard (Admin Only)

* Sales overview (daily, weekly, monthly)
* Top-selling products
* Low-stock items
* Supplier expense tracking
* Profit = Total Sales − Supplier Costs
* Charts: Revenue trend, product performance, category pie chart

### 4. Inventory Management (Admin Only)

*   Admin can view, add, edit, and delete product inventory.
*   Update stock quantities and cost prices manually.
*   Manage product details (name, category, selling price).

### 5. Authentication and Authorization

*   User login (Admin, Operator).
*   JWT-based authentication.
*   Role-based authorization for API endpoints and frontend routes.
*   User registration.