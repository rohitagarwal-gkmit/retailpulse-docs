# API Design

The API is designed following RESTful principles to provide a logical, hierarchical structure for managing a multi-store medical wholesale business. Resources are nested where it makes sense (e.g., a bill belongs to a store).

---

### Authentication Endpoints
*No change here. Authentication is the entry point.*

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/auth/login` | User login to get a JWT access token. |
| `POST` | `/auth/logout` | User logout (invalidates token). |

---

### Store Endpoints
*For Company Admins to manage the store network.*

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/stores` | Create a new store. |
| `GET` | `/stores` | List all stores in the company. |
| `GET` | `/stores/{store_id}` | Get details for a specific store. |
| `PATCH`| `/stores/{store_id}` | Update a store's details. |

---

### User Management Endpoints
*For Company Admins to manage users.*

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/users` | Create a new user (can assign a role and store). |
| `GET` | `/users` | List all users (can filter by store, role). |
| `GET` | `/users/{user_id}` | Get details for a specific user. |
| `PATCH`| `/users/{user_id}` | Update a user's details (e.g., role, store). |
| `DELETE`| `/users/{user_id}`| Deactivate a user account. |

---

### Medicine Master Endpoints
*For managing the company-wide medicine catalog.*

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/medicines` | Create a new medicine in the master catalog. |
| `GET` | `/medicines` | List all medicines in the master catalog. |
| `GET` | `/medicines/{medicine_id}` | Get details for a specific master medicine. |
| `PATCH`| `/medicines/{medicine_id}` | Update a master medicine's details. |

---

### Inventory Endpoints
*The core of inventory management. Granular control over stock.*

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/stores/{store_id}/inventory-items` | Add a new inventory item (a new batch) to a store. |
| `GET` | `/stores/{store_id}/inventory-items` | Get all inventory items for a specific store. |
| `GET` | `/inventory-items/{item_id}` | Get details of a specific inventory item (batch). |
| `PATCH`| `/inventory-items/{item_id}` | Update an inventory item (e.g., adjust quantity, change price). |
| `POST` | `/inventory-items/movements` | Log an internal movement of stock within a store. |
| `POST` | `/inventory-transfers` | Initiate a transfer of stock between two stores. |
| `PATCH`| `/inventory-transfers/{transfer_id}` | Update the status of a transfer (e.g., confirm receipt). |

---

### Bill Endpoints
*Store-specific endpoints for creating and viewing bills.*

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/stores/{store_id}/bills` | Create a new bill for a specific store. |
| `GET` | `/stores/{store_id}/bills` | List all bills for a specific store. |
| `GET` | `/bills/{bill_id}` | Get details for a specific bill (accessible across stores by Admins). |

---

### Analytics Endpoints
*Flexible endpoints for company-wide or store-specific insights.*

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/analytics/sales` | Get sales report data. Can be filtered by `store_id`. If no filter, returns company-wide data. |
| `GET` | `/analytics/inventory` | Get inventory reports (e.g., stock valuation, expiring soon). Can be filtered by `store_id`. |
| `GET` | `/analytics/performance` | Get performance metrics (e.g., top medicines). Can be filtered by `store_id`. |