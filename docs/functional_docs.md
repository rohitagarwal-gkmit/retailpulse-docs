# Functional Documentation

## User Roles

| Role | Access | What they can do |
|---|---|---|
| Admin | Full | See reports, profits, stock trends, add/change/delete products, view all bills, manage users |
| Store Manager | Limited | Create bills, view stock, download PDF bills, search products |

---

## System Modules Overview

RetailPulse has three main parts that help medical wholesale businesses.

---

## Module 1: Bill Generation

### Purpose
To help store managers make bills quickly and correctly.

### Features

#### 1.1 Product Search and Selection
- Search: Find medicines by name, maker, or type.
- Product Details: See stock and price when picking a product.
- Multi-Product: Add many products to one bill.

#### 1.2 Real-time Stock Validation
- Availability: Check if a product is in stock before adding to a bill.
- Quantity: Make sure the amount asked for is not more than what's available.

#### 1.3 Bill Creation Form
- Customer Info: Add customer name, contact.
- Product List: Shows product name, unit price, quantity, and total price.
- Calculations: Shows subtotal, discount, tax, and grand total.

#### 1.4 Bill Numbering and Tracking
- Unique Numbers: Bills get an automatic, unique number.
- Date/Time: Records when the bill was made.
- Created By: Shows which manager made the bill.

#### 1.5 PDF Generation
Bills are made in a professional PDF format with the store logo. Includes store info, item list, and payment summary. You can download the PDF instantly.

#### 1.6 Automatic Inventory Update
- Stock Deduction: Stock amounts are automatically reduced.
- Sales Log: Every sale is recorded for reports.

---

## Module 2: Inventory Management

### Purpose
To give admins full control over products and stock, with alerts for low or expiring items.

### Features

#### 2.1 Product Creation
- Basic Info: Add product name, category, manufacturer, and description.
- Pricing: Set cost price, selling price, and see profit margin.

#### 2.2 View Inventory
- List View: See all products in a sortable table with key details.
- Search and Filter: Find products by name, maker, category, or stock status.

#### 2.3 Update Inventor
- Edit Details: Change product prices, reorder levels, or descriptions.
- Stock Adjustments: Manually add or reduce stock, with reasons and a record of who did it.

#### 2.4 Delete Product
- Delete: permanently removed.

---

## Module 3: Analytics Dashboard

### Purpose
To give admins real-time business insights for smart decisions.

### Features

#### 3.1 Sales Analytics
- Revenue: Shows total revenue and trends over time.
- Bills: Shows number of bills, average bill value, and peak hours.
- Profit: Shows total profit, profit margin, and trends.

#### 3.2 Product Performance
- Best Sellers: Lists top products by sales, revenue, and profit.
- Slow Movers: Identifies products with low sales.
- Profitability: Shows products with high profit margins or those losing money.

#### 3.3 Inventory Insights
- Stock Summary: Shows total inventory value, unique products, and low/out-of-stock items.
- Categories: Shows stock and sales by category.

#### 3.4 Time Range Filters
- Quick Views: See data for today, this week, this month, or this year.
- Custom Dates: Pick your own start and end dates.

#### 3.5 Visual Dashboards
- Charts: Uses line, bar, and pie charts for easy understanding.
- Interactive: Hover for details, click to see more.

---

## Module 4: Authentication and Authorization

### Purpose
To secure the system and ensure users only access what they are allowed to.

### Features

#### 4.1 User Registration
- Admin Only: Only admins can create new user accounts.
- User Details: Admins add name, username, password, and role.

#### 4.2 User Login
- Methods: Log in with username and password.
- Session: Manages user sessions and automatic logouts.

#### 4.3 Role-Based Access Control
- Manager Role: Can create bills, view stock, and see their own bill history.
- Admin Role: Has all manager permissions plus full control over inventory, analytics, and user management.