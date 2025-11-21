# Edge Cases and Error Handling

## Overview

This document explains how RetailPulse handles common problems and unexpected situations.

---

## 1. Login and Access Problems

- **Problem**: User tries to log in with no username or password.

  - **Solution**: The system shows a "Username and password are required" message.

- **Problem**: User enters the wrong username or password.

  - **Solution**: The system shows an "Invalid username or password" message.

- **Problem**: A deactivated user tries to log in.

  - **Solution**: The system shows a "Your account has been deactivated" message.

- **Problem**: A `Store Manager` tries to access a `Company Admin`-only page.
  - **Solution**: The system shows an "Insufficient permissions" message and denies access.

---

## 2. Bill Creation Problems

- **Problem**: User tries to create a bill with no products.

  - **Solution**: The system requires at least one product to be in the bill.

- **Problem**: A product is not found in the store's inventory or is inactive.

  - **Solution**: The system shows a "Product not found or inactive" message.

- **Problem**: The quantity requested is more than the available stock in the selected batch.

  - **Solution**: The system shows an error message like "Only 5 units available in this batch".

- **Problem**: The quantity is zero or a negative number.

  - **Solution**: The system requires the quantity to be greater than zero.

- **Problem**: The database connection is lost while creating a bill.
  - **Solution**: The entire transaction is cancelled (rolled back) to prevent partial data. The user is asked to try again.

---

## 3. Inventory Management Problems

- **Problem**: `Company Admin` tries to create a product with a name that already exists.

  - **Solution**: The system shows a "Product with this name already exists" message.

- **Problem**: `Stockist` tries to adjust stock to a negative quantity.
  - **Solution**: The system shows a "Stock quantity cannot be negative" message. The database has a `CHECK` constraint to enforce this.

---

## 4. User Management Problems

- **Problem**: `Company Admin` tries to create a user with a username that already exists.

  - **Solution**: The system shows a "Username already exists" message.

- **Problem**: `Company Admin` tries to deactivate their own account.
  - **Solution**: The system prevents this to ensure there's always at least one active super admin.

---

## 5. Multi-Store and Advanced Inventory Edge Cases

- **Problem**: A `Sales` user tries to sell a product from a batch that has expired.

  - **Solution**: The system should not show expired batches in the search results on the billing page. If an API call is made directly, the backend should reject the request with a "Cannot sell from expired batch" error.

- **Problem**: Two `Sales` users try to sell the last item of a batch at the same time (a race condition).

  - **Solution**: The system uses database-level transaction isolation. The first transaction to commit will succeed. The second transaction will fail when it attempts to update the quantity, as the stock will already be zero. It will receive an error like "Insufficient stock".

- **Problem**: A branch store loses internet connection to the central server.

  - **Solution**: In the current design, the system requires a live connection to the central database. If the connection is lost, the store's terminal will not be able to create new bills or look up inventory. (A future enhancement could be a limited "offline mode" that syncs data once the connection is restored).

- **Problem**: A `Company Admin` reassigns a `Store Manager` from Store A to Store B.
  - **Solution**: The user's `store_id` is updated in the `users` table. The change should take effect on their next login. When they log in again, their session data will be for Store B, and they will no longer have access to Store A's data or settings.
