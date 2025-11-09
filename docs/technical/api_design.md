# API Design

The API is designed in a RESTful way. Here are the main endpoints.

### Authentication Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/auth/login` | User login to get an access token. |

### Bill Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/bills` | Create a new bill. |
| `GET` | `/bills` | List bills (Admins see all, Managers see their own). |
| `GET` | `/bills/:id` | Get details for one bill. |

### Product Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/products` | List all products. |
| `POST` | `/products` | Create a new product (Admin only). |
| `PATCH` | `/products/:id` | Update a product (Admin only). |
| `DELETE` | `/products/:id` | Delete a product (Admin only). |

### Analytics Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/analytics/dashboard` | Get main dashboard metrics (Admin only). |
| `GET` | `/analytics/sales` | Get sales report data (Admin only). |

### User Management Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/users` | List all users (Admin only). |
| `POST` | `/users` | Create a new user (Admin only). |
| `PATCH` | `/users/:id` | Update a user (Admin only). |
| `DELETE` | `/users/:id` | Deactivate a user (Admin only). |