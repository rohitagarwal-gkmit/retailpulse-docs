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

-   **Problem**: A `Store Manager` tries to access a `Company Admin`-only page.
    -   **Solution**: The system shows an "Insufficient permissions" message and denies access.

---

## 2. Bill Creation Problems

-   **Problem**: User tries to create a bill with no medicines.
    -   **Solution**: The system requires at least one medicine to be in the bill.

-   **Problem**: A medicine is not found in the store's inventory or is inactive.
    -   **Solution**: The system shows a "Medicine not found or inactive" message.

-   **Problem**: The quantity requested is more than the available stock in the selected batch.
    -   **Solution**: The system shows an error message like "Only 5 units available in this batch".

-   **Problem**: The quantity is zero or a negative number.
    -   **Solution**: The system requires the quantity to be greater than zero.

-   **Problem**: The database connection is lost while creating a bill.
    -   **Solution**: The entire transaction is cancelled (rolled back) to prevent partial data. The user is asked to try again.

---

## 3. Inventory Management Problems

-   **Problem**: `Company Admin` tries to create a medicine with a name that already exists.
    -   **Solution**: The system shows a "Medicine with this name already exists" message.

-   **Problem**: `Stockist` tries to adjust stock to a negative quantity.
    -   **Solution**: The system shows a "Stock quantity cannot be negative" message. The database has a `CHECK` constraint to enforce this.

-   **Problem**: A user adjusts stock without giving a reason.
    -   **Solution**: The system requires a reason for all manual stock adjustments to maintain a clear audit trail.

---

## 4. Analytics Problems

-   **Problem**: There is no sales data for the selected date range in a specific store.
    -   **Solution**: The system shows a "No data available for this time period" message.

-   **Problem**: The start date is after the end date in a custom date range filter.
    -   **Solution**: The system shows a "Start date must be before end date" message.

---

## 5. User Management Problems

-   **Problem**: `Company Admin` tries to create a user with a username that already exists.
    -   **Solution**: The system shows a "Username already exists" message.

-   **Problem**: `Company Admin` tries to deactivate their own account.
    -   **Solution**: The system prevents this to ensure there's always at least one active super admin.

---

## 6. Multi-Store and Advanced Inventory Edge Cases

-   **Problem**: A `Clerk` tries to sell a medicine from a batch that has expired.
    -   **Solution**: The system should not show expired batches in the search results on the billing page. If an API call is made directly, the backend should reject the request with a "Cannot sell from expired batch" error.

-   **Problem**: Two clerks try to sell the last item of a batch at the same time (a race condition).
    -   **Solution**: The system uses database-level transaction isolation. The first transaction to commit will succeed. The second transaction will fail when it attempts to update the quantity, as the stock will already be zero. It will receive an error like "Insufficient stock".

-   **Problem**: A stock transfer is sent from Store A, but Store B claims it was never received or the quantity is wrong.
    -   **Solution**: The transfer remains in an "In Transit" state until the receiving store confirms it. This creates a clear paper trail. If there's a dispute, a `Company Admin` can manually intervene to reconcile the inventory, and the discrepancy is logged.

-   **Problem**: A branch store loses internet connection to the central server.
    -   **Solution**: In the current design, the system requires a live connection to the central database. If the connection is lost, the store's terminal will not be able to create new bills or look up inventory. (A future enhancement could be a limited "offline mode" that syncs data once the connection is restored).

-   **Problem**: A `Company Admin` reassigns a `Store Manager` from Store A to Store B.
    -   **Solution**: The user's `store_id` is updated in the `users` table. The change should take effect on their next login. When they log in again, their session data will be for Store B, and they will no longer have access to Store A's data or settings.