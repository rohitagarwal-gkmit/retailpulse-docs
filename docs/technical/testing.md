# Testing Documentation

## Testing Frameworks

- **Backend**: Pytest v9.0.0 (pytest-asyncio v1.3.0, pytest-cov v7.0.0)
- **Frontend**: Jest v30.2.0

## Test Cases Overview

I've outlined the key test cases for the RetailPulse application covering critical user flows from authentication to bill generation. These tests include both positive (happy path) and negative (error handling) scenarios.

### Backend Test Cases

#### 1. Health Check API
**Endpoint:** `GET /health`
- **Positive Test**: Verify endpoint returns 200 status with `{"status": "ok"}`
- **Negative Test**: Verify no database changes occur during health check

---

#### 2. User Authentication
**Endpoint:** `POST /auth/login`
- **Positive Test**: Valid credentials return 200 with access_token
- **Negative Test**: Invalid credentials return 401 with error message
- **Negative Test**: Missing credentials return 400 validation error

**Endpoint:** `POST /auth/register`
- **Positive Test**: Valid user registration returns 201 without password in response
- **Negative Test**: Duplicate email returns 409 conflict error
- **Negative Test**: Weak password returns 400 with validation message

---

#### 3. Inventory Management
**Endpoint:** `POST /api/items`
- **Positive Test**: Valid item payload creates item and returns 201 with item ID
- **Negative Test**: Duplicate SKU returns 400 error
- **Negative Test**: Negative stock quantity returns 400 validation error

**Endpoint:** `GET /api/items`
- **Positive Test**: Returns paginated list of items with 200
- **Negative Test**: Invalid page number returns 400 or returns empty list

---

#### 4. Bill Generation
**Function:** `calculate_line_total(quantity, unit_price, discount)`
- **Positive Test**: Correct calculation: (quantity × unit_price) - discount
- **Negative Test**: Zero or negative quantity returns zero or validation error

**Endpoint:** `POST /api/bills`
- **Positive Test**: Valid bill payload creates bill and returns 201 with bill ID
- **Negative Test**: Bill with insufficient stock returns 400 with error message

---

### Frontend Test Cases

#### 5. Login Flow
**Component:** `LoginPage`
- **Positive Test**: Valid credentials redirect to dashboard and store token
- **Negative Test**: Invalid credentials show error message and do not redirect
- **Negative Test**: Empty form submission shows validation errors

---

#### 6. Dashboard
**Component:** `Dashboard`
- **Positive Test**: Dashboard loads and displays stats widgets with data
- **Negative Test**: API failure shows error message with retry option

---

#### 7. Create Bill Page
**Component:** `CreateBillPage`
- **Positive Test**: Adding line items updates subtotal and grand total correctly
- **Negative Test**: Submitting bill with empty items shows validation error

---

## Next Steps

1. Implement these initial test cases
2. Run tests and fix any failures
3. Gradually expand coverage to other modules (stock transfers, user management)
4. Add integration tests for complete workflows