# Development Workflow & Automated Testing

This document describes the development workflow, branching strategy, code merge process, and automated testing procedures for the RetailPulse project.

## Branch Structure

The project maintains the following branch hierarchy:

```output
main (production)
  └── staging (pre-production)
       └── dev (development)
            ├── feature/* (feature branches)
            ├── feat/* (feature branches)
            └── fix/* (bugfix branches)
```

### Current Active Branches

- **main**: Production-ready code
- **staging**: Pre-production testing environment
- **dev**: Active development branch
- **feat/ci-cd-setup**: CI/CD pipeline implementation
- **feat/ui-ux-enhancements-forms**: UI/UX improvements
- **feature/apis-fixes-and-updates**: API enhancements and fixes
- **feature/company-admin-dashboard**: Admin dashboard features
- **feature/database-schema**: Database structure changes
- **feature/project-setup**: Initial project configuration
- **feature/tests**: Test suite development

## Code Merge Workflow

### 1. Feature Branch Creation

```bash
# Create a new feature branch from dev
git checkout dev
git pull origin dev
git checkout -b feature/your-feature-name
```

### 2. Development Process

- Make incremental commits with clear, descriptive messages
- Follow conventional commit format: `feat:`, `fix:`, `refactor:`, `chore:`, `docs:`
- Keep commits focused on single logical changes
- Write tests for new features

### 3. Code Quality Checks

Before creating a pull request:

```bash
# Backend: Run tests and linting
pytest --cov=app --cov-report=term-missing --cov-report=html -v

# Frontend: Run tests and linting
npm test -- --coverage
npm run lint
```

### 4. Pull Request Process

1. **Push to Remote**: Push your feature branch to GitHub

   ```bash
   git push origin feature/your-feature-name
   ```

2. **Create Pull Request**:

    - Target branch: `dev`
    - Provide clear title and description
    - Reference related issues
    - Add reviewers

3. **Code Review**:

   - Address reviewer feedback
   - Make requested changes
   - Update PR with new commits

4. **CI/CD Checks**:
    - Automated tests must pass
    - Code coverage requirements must be met
    - Security scans (Semgrep) must pass

5. **Merge**:
    - Squash and merge for clean history
    - Delete feature branch after merge

### 5. Merge Flow Diagram

```output
feature/project-setup ──┐
                        ├──> dev ──┐
feature/database-schema ┘           │
                                    ├──> staging ──> main
feature/tests ──────────┐           │
                        ├──> dev ──┘
feat/ci-cd-setup ───────┘
```

## Recent Merge History

### Development Branch Merges

1. **feat/ci-cd-setup → dev** (PR #9)

    - Added PostgreSQL service to CI/CD pipeline
    - Implemented database migration steps
    - Added automatic deployment to EC2

2. **feature/apis-fixes-and-updates → feat/ui-ux-enhancements-forms** (PR #8)

    - Implemented user management endpoints
    - Enhanced authentication and billing features
    - Refactored role management

3. **feature/tests → dev** (PR #7)

    - Added comprehensive test suite
    - Configured test database
    - Implemented test fixtures

4. **feature/company-admin-dashboard → dev** (Multiple PRs: #2, #3, #4, #5, #6)

    - Added patch endpoints for bills and categories
    - Implemented password change functionality
    - Added comprehensive CRUD operations

5. **feature/database-schema → dev** (PR #2)

    - Migrated to UUID primary keys
    - Added database seeding script
    - Fixed schema relationships

6. **feature/project-setup → dev** (PR #1)
    - Initial project structure
    - Added pytest configuration
    - Created requirements.txt

## Automated Testing

### Backend Testing (Python/FastAPI)

#### Test Configuration

**Framework**: pytest 9.0.0 with pytest-cov 7.0.0 and pytest-asyncio 1.3.0

#### Test Execution Results

```output
============================================ test session starts ============================================
platform darwin -- Python 3.13.7, pytest-9.0.0, pluggy-1.6.0 -- /Users/rohitagarwal/retailpulse/.venv/bin/python
cachedir: .pytest_cache
rootdir: /Users/rohitagarwal/retailpulse/retailpulse-backend
configfile: pyproject.toml
testpaths: tests
plugins: anyio-4.11.0, asyncio-1.3.0, cov-7.0.0
asyncio: mode=Mode.AUTO, debug=False, asyncio_default_fixture_loop_scope=function, asyncio_default_test_loop_scope=function
collected 61 items

tests/test_auth.py::test_login_success PASSED                                                         [  1%]
tests/test_auth.py::test_login_invalid_username PASSED                                                [  3%]
tests/test_auth.py::test_login_invalid_password PASSED                                                [  4%]
tests/test_auth.py::test_login_missing_fields PASSED                                                  [  6%]
tests/test_auth.py::test_login_empty_credentials PASSED                                               [  8%]
tests/test_bills.py::test_get_bills PASSED                                                            [  9%]
tests/test_categories.py::test_create_category PASSED                                                 [ 11%]
tests/test_categories.py::test_create_category_invalid_data PASSED                                    [ 13%]
tests/test_categories.py::test_get_categories_empty PASSED                                            [ 14%]
tests/test_categories.py::test_get_categories_with_data PASSED                                        [ 16%]
tests/test_categories.py::test_get_category_by_id PASSED                                              [ 18%]
tests/test_categories.py::test_get_category_not_found PASSED                                          [ 19%]
tests/test_categories.py::test_update_category PASSED                                                 [ 21%]
tests/test_categories.py::test_update_category_not_found PASSED                                       [ 22%]
tests/test_categories.py::test_delete_category PASSED                                                 [ 24%]
tests/test_categories.py::test_delete_category_not_found PASSED                                       [ 26%]
tests/test_health.py::test_health_check PASSED                                                        [ 27%]
tests/test_inventories.py::test_create_inventory PASSED                                               [ 29%]
tests/test_inventories.py::test_create_inventory_invalid_product PASSED                               [ 31%]
tests/test_inventories.py::test_create_inventory_invalid_store PASSED                                 [ 32%]
tests/test_inventories.py::test_get_inventories_empty PASSED                                          [ 34%]
tests/test_inventories.py::test_get_inventories_with_data PASSED                                      [ 36%]
tests/test_inventories.py::test_get_inventory_by_id PASSED                                            [ 37%]
tests/test_inventories.py::test_get_inventory_not_found PASSED                                        [ 39%]
tests/test_inventories.py::test_update_inventory PASSED                                               [ 40%]
tests/test_inventories.py::test_update_inventory_not_found PASSED                                     [ 42%]
tests/test_inventories.py::test_delete_inventory PASSED                                               [ 44%]
tests/test_inventories.py::test_delete_inventory_not_found PASSED                                     [ 45%]
tests/test_products.py::test_create_product PASSED                                                    [ 47%]
tests/test_products.py::test_create_product_invalid_category PASSED                                   [ 49%]
tests/test_products.py::test_create_product_invalid_data PASSED                                       [ 50%]
tests/test_products.py::test_get_products_empty PASSED                                                [ 52%]
tests/test_products.py::test_get_products_with_data PASSED                                            [ 54%]
tests/test_products.py::test_get_product_by_id PASSED                                                 [ 55%]
tests/test_products.py::test_get_product_not_found PASSED                                             [ 57%]
tests/test_products.py::test_update_product PASSED                                                    [ 59%]
tests/test_products.py::test_update_product_not_found PASSED                                          [ 60%]
tests/test_products.py::test_delete_product PASSED                                                    [ 62%]
tests/test_products.py::test_delete_product_not_found PASSED                                          [ 63%]
tests/test_stores.py::test_create_store PASSED                                                        [ 65%]
tests/test_stores.py::test_create_store_invalid_data PASSED                                           [ 67%]
tests/test_stores.py::test_get_stores_empty PASSED                                                    [ 68%]
tests/test_stores.py::test_get_stores_with_data PASSED                                                [ 70%]
tests/test_stores.py::test_get_store_by_id PASSED                                                     [ 72%]
tests/test_stores.py::test_get_store_not_found PASSED                                                 [ 73%]
tests/test_stores.py::test_update_store PASSED                                                        [ 75%]
tests/test_stores.py::test_update_store_not_found PASSED                                              [ 77%]
tests/test_stores.py::test_update_store_invalid_data PASSED                                           [ 78%]
tests/test_stores.py::test_delete_store PASSED                                                        [ 80%]
tests/test_stores.py::test_delete_store_not_found PASSED                                              [ 81%]
tests/test_users.py::test_create_user PASSED                                                          [ 83%]
tests/test_users.py::test_create_user_duplicate_username PASSED                                       [ 85%]
tests/test_users.py::test_create_user_invalid_data PASSED                                             [ 86%]
tests/test_users.py::test_get_users_empty PASSED                                                      [ 88%]
tests/test_users.py::test_get_users_with_data PASSED                                                  [ 90%]
tests/test_users.py::test_get_user_by_id PASSED                                                       [ 91%]
tests/test_users.py::test_get_user_not_found PASSED                                                   [ 93%]
tests/test_users.py::test_update_user PASSED                                                          [ 95%]
tests/test_users.py::test_update_user_not_found PASSED                                                [ 96%]
tests/test_users.py::test_delete_user PASSED                                                          [ 98%]
tests/test_users.py::test_delete_user_not_found PASSED                                                [100%]

============================================== tests coverage ===============================================
_____________________________ coverage: platform darwin, python 3.13.7-final-0 ______________________________

Name                               Stmts   Miss  Cover   Missing
----------------------------------------------------------------
app/core/config.py                    10      0   100%
app/core/security.py                   9      0   100%
app/db/seed.py                        33     33     0%   1-70
app/db/session.py                     13      5    62%   13-18
app/main.py                           26      1    96%   40
app/models/bill.py                    24      0   100%
app/models/bill_product.py            19      0   100%
app/models/category.py                15      0   100%
app/models/inventory.py               25      0   100%
app/models/inventory_movement.py      20      0   100%
app/models/permission.py              14      0   100%
app/models/product.py                 20      0   100%
app/models/role.py                    15      0   100%
app/models/role_permission.py         16      0   100%
app/models/store.py                   17      0   100%
app/models/user.py                    19      0   100%
app/models/user_role.py               16      0   100%
app/models/user_store.py              16      0   100%
app/routers/auth.py                   38      7    82%   38, 86-95
app/routers/bills.py                 187    152    19%   21-43, 61-72, 77-111, 116-146, 151-157, 167-192, 204-239, 250-285, 295-311
app/routers/categories.py             53      8    85%   60-69
app/routers/inventories.py            76     16    79%   34, 36, 44-46, 78-82, 85-89, 93, 100, 102
app/routers/products.py               59      2    97%   96, 110
app/routers/roles.py                  11      2    82%   13-14
app/routers/stores.py                 69     14    80%   19, 30-38, 90-97, 116-118
app/routers/users.py                 239    157    34%   32, 73-81, 89-133, 205-207, 219-226, 234-288, 295-331, 360-378, 386-406, 414-464, 489, 500-508
app/schemas/auth.py                   10      0   100%
app/schemas/bill.py                   62     12    81%   25-27, 32-34, 56-58, 63-65
app/schemas/bill_product.py           52     12    77%   20-22, 27-29, 47-49, 54-56
app/schemas/category.py               16      0   100%
app/schemas/inventory.py              41      2    95%   22, 45
app/schemas/product.py                22      0   100%
app/schemas/roles.py                   8      0   100%
app/schemas/store.py                  14      0   100%
app/schemas/user.py                   32      0   100%
app/util/current_user.py              24     14    42%   16-37
----------------------------------------------------------------
TOTAL                               1340    437    67%
Coverage HTML written to dir htmlcov
============================================ 61 passed in 2.45s =============================================
```

#### Test Summary

- **Total Tests**: 61
- **Passed**: 61 (100%)
- **Failed**: 0
- **Overall Coverage**: 67%
- **Execution Time**: 2.45s

#### Test Modules

1. **test_auth.py** (5 tests)

    - Login success/failure scenarios
    - Invalid credentials handling
    - Missing fields validation

2. **test_bills.py** (1 test)

    - Bill retrieval functionality

3. **test_categories.py** (10 tests)

    - CRUD operations for categories
    - Validation and error handling

4. **test_inventories.py** (11 tests)

    - Inventory management operations
    - Foreign key validation

5. **test_products.py** (11 tests)

    - Product CRUD operations
    - Category relationship validation

6. **test_stores.py** (11 tests)

    - Store management operations
    - Data validation

7. **test_users.py** (11 tests)

    - User management operations
    - Duplicate username prevention

8. **test_health.py** (1 test)
    - API health check endpoint

#### Coverage by Module

| Module                      | Coverage | Status               |
| --------------------------- | -------- | -------------------- |
| app/core/config.py          | 100%     | ✅ Excellent         |
| app/core/security.py        | 100%     | ✅ Excellent         |
| app/models/\*               | 100%     | ✅ Excellent         |
| app/schemas/\*              | 77-100%  | ✅ Good              |
| app/routers/products.py     | 97%      | ✅ Excellent         |
| app/main.py                 | 96%      | ✅ Excellent         |
| app/schemas/inventory.py    | 95%      | ✅ Excellent         |
| app/routers/categories.py   | 85%      | ✅ Good              |
| app/routers/auth.py         | 82%      | ✅ Good              |
| app/routers/roles.py        | 82%      | ✅ Good              |
| app/schemas/bill.py         | 81%      | ✅ Good              |
| app/routers/stores.py       | 80%      | ✅ Good              |
| app/routers/inventories.py  | 79%      | ⚠️ Needs Improvement |
| app/schemas/bill_product.py | 77%      | ⚠️ Needs Improvement |
| app/db/session.py           | 62%      | ⚠️ Needs Improvement |
| app/util/current_user.py    | 42%      | ❌ Low Coverage      |
| app/routers/users.py        | 34%      | ❌ Low Coverage      |
| app/routers/bills.py        | 19%      | ❌ Low Coverage      |
| app/db/seed.py              | 0%       | ❌ Not Tested        |

### Frontend Testing (React/Jest)

**Framework**: Jest 30.2.0 with Testing Library

**Configuration** (`jest.config.js`):

```javascript
export default {
  testEnvironment: "jsdom",
  setupFilesAfterEnv: ["<rootDir>/jest.setup.js"],
  moduleNameMapper: {
    "\\.(css|less|scss|sass)$": "identity-obj-proxy",
    "\\.(jpg|jpeg|png|gif|svg)$": "<rootDir>/__mocks__/fileMock.js",
  },
  transform: {
    "^.+\\.[tj]sx?$": [
      "babel-jest",
      {
        presets: [
          ["@babel/preset-env", { targets: { node: "current" } }],
          ["@babel/preset-react", { runtime: "automatic" }],
        ],
      },
    ],
  },
  collectCoverageFrom: [
    "src/**/*.{js,jsx}",
    "!src/main.jsx",
    "!src/**/*.test.{js,jsx}",
  ],
  coverageThreshold: {
    global: {
      branches: 50,
      functions: 50,
      lines: 50,
      statements: 50,
    },
  },
};
```

#### Test Execution Results - Frontend

```out
 PASS  src/tests/init.test.jsx
 PASS  src/tests/ProtectedRoute.test.jsx
 PASS  src/tests/Login.test.jsx

Test Suites: 3 passed, 3 total
Tests:       12 passed, 12 total
Snapshots:   0 total
Time:        4.17 s
```

#### Test Summary - Frontend

- **Total Test Suites**: 3
- **Total Tests**: 12
- **Passed**: 12 (100%)
- **Failed**: 0
- **Overall Coverage**: 3.64%
- **Execution Time**: 4.17s

#### Test Modules - Frontend

1. **init.test.jsx** (1 test)

    - App component health check

2. **ProtectedRoute.test.jsx** (6 tests)

    - Loading state handling
    - Authentication redirects
    - Role-based access control
    - Authorization checks

3. **Login.test.jsx** (5 tests)
    - Form rendering
    - User input handling
    - Login success with role-based redirects
    - Error message display

#### Coverage Analysis

**Current Coverage**: 3.64% (statements), 2.24% (branches), 2.77% (functions), 3.74% (lines)

**Coverage Thresholds**: 50% (global requirement - NOT MET)

| Component Category   | Coverage | Status               |
| -------------------- | -------- | -------------------- |
| Core Components      | 30.3%    | ❌ Below Threshold   |
| Authentication       | 45.94%   | ❌ Below Threshold   |
| Protected Routes     | 100%     | ✅ Excellent         |
| Constants            | 100%     | ✅ Excellent         |
| Contexts             | 65.51%   | ⚠️ Needs Improvement |
| Billing Components   | 0%       | ❌ Not Tested        |
| Dashboard Components | 0%       | ❌ Not Tested        |
| Forms                | 0%       | ❌ Not Tested        |
| Services             | 0%       | ❌ Not Tested        |
| Utilities            | 0%       | ❌ Not Tested        |

#### Areas Requiring Additional Tests

1. **High Priority** (0% coverage):

    - Billing components and services
    - Dashboard components
    - Form components
    - API services
    - Utility functions

2. **Medium Priority** (Low coverage):
    - Layout component
    - Authentication context (expand coverage)
    - Page components

## Continuous Integration/Continuous Deployment (CI/CD)

### CI/CD Pipeline (GitHub Actions)

The project uses GitHub Actions for automated testing and deployment:

#### Pipeline Triggers

- Push to `dev` branch

#### Pipeline Steps

1. **Code Checkout**: Clone repository
2. **Environment Setup**:
    - Install Python 3.13
    - Install Node.js 18+
    - Install dependencies
3. **Database Setup**:
    - Start PostgreSQL service
    - Run migrations
4. **Testing**:
    - Run backend tests with coverage
    - Run frontend tests with coverage
    - Generate coverage reports
5. **Security Scanning**:
    - Run Semgrep security analysis
    - Check for vulnerabilities
6. **Deployment** (on push to dev):
    - Deploy to EC2 instance
    - Restart services

## Best Practices

### Code Quality

1. **Write Tests First**: Follow TDD principles when possible
2. **Maintain Coverage**: Aim for 80%+ code coverage
3. **Test Edge Cases**: Include error handling and boundary conditions
4. **Use Fixtures**: Reuse test data with pytest fixtures
5. **Mock External Dependencies**: Isolate unit tests from external services

### Git Workflow

1. **Commit Messages**: Use conventional commit format

   ```out
   feat: add user authentication endpoint
   fix: resolve inventory update bug
   refactor: improve database query performance
   docs: update API documentation
   test: add tests for billing module
   ```

2. **Branch Naming**: Use descriptive branch names

   ```out
   feature/add-user-roles
   fix/inventory-calculation-bug
   refactor/database-schema
   ```

3. **Pull Request**: Include:
   - Clear description of changes
   - Related issue numbers
   - Test results
   - Screenshots (for UI changes)
