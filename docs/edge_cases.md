# Edge Cases and Error Handling

## Overview

This document explains how RetailPulse handles common problems and unexpected situations.

---

## 1. Login and Access Problems

-   **Problem**: User tries to log in with no username or password.
    -   **Solution**: The system shows a "Username and password are required" message.

-   **Problem**: User enters the wrong username or password.
    -   **Solution**: The system shows an "Invalid username or password" message.

-   **Problem**: A deactivated user tries to log in.
    -   **Solution**: The system shows a "Your account has been deactivated" message.

-   **Problem**: A manager tries to access an admin-only page.
    -   **Solution**: The system shows an "Insufficient permissions" message.

---

## 2. Bill Creation Problems

-   **Problem**: User tries to create a bill with no products.
    -   **Solution**: The system requires at least one product to be in the bill.

-   **Problem**: A product is not found or is inactive.
    -   **Solution**: The system shows a "Product not found or inactive" message.

-   **Problem**: The quantity requested is more than the available stock.
    -   **Solution**: The system shows an error message like "Only 5 units available".

-   **Problem**: The quantity is zero or a negative number.
    -   **Solution**: The system requires the quantity to be greater than zero.

-   **Problem**: The final bill total is zero.
    -   **Solution**: The system requires the bill total to be greater than zero.

-   **Problem**: The database connection is lost while creating a bill.
    -   **Solution**: The entire transaction is cancelled to prevent partial data. The user is asked to try again.

---

## 3. Inventory Management Problems

-   **Problem**: Admin tries to create a product with a name that already exists.
    -   **Solution**: The system shows a "Product with this name already exists" message.

-   **Problem**: Admin tries to create a product with a negative price.
    -   **Solution**: The system requires the price to be a positive number.

-   **Problem**: Admin tries to delete a product that has been sold in past bills.
    -   **Solution**: The system prevents deletion to keep historical sales data intact.

-   **Problem**: Admin tries to adjust stock to a negative quantity.
    -   **Solution**: The system shows a "Stock cannot be negative" message.

-   **Problem**: Admin adjusts stock without giving a reason.
    -   **Solution**: The system requires a reason for all stock adjustments to maintain a clear history.

---

## 4. Analytics Problems

-   **Problem**: There is no sales data for the selected date range.
    -   **Solution**: The system shows a "No data available for this time period" message.

-   **Problem**: The start date is after the end date in a custom date range.
    -   **Solution**: The system shows a "Start date must be before end date" message.

---

## 5. User Management Problems

-   **Problem**: Admin tries to create a user with a username that already exists.
    -   **Solution**: The system shows a "Username already exists" message.

-   **Problem**: Admin tries to deactivate their own account.
    -   **Solution**: The system prevents this to ensure there's always an active admin.

-   **Problem**: Admin tries to deactivate the last remaining admin account.
    -   **Solution**: The system prevents this and asks the admin to promote another user first.

---

## Summary

This document helps make sure:

- The system is strong and handles errors well.
- Users get clear messages when something goes wrong.
- The data stays correct and consistent.