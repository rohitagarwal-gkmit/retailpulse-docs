# Continuous Integration/Continuous Deployment (CI/CD)

This document describes the CI/CD pipelines for the RetailPulse project, which uses GitHub Actions for automated testing and deployment.

## Overview

The RetailPulse project uses separate CI/CD pipelines for the backend (Python/FastAPI) and frontend (React) applications. Both pipelines are triggered automatically on push to the `dev` branch or can be triggered manually using workflow dispatch.

## Backend CI/CD Pipeline

### Pipeline Configuration for Backend

- **Workflow File**: `.github/workflows/deploy.yml`

### Triggers for Backend

1. **Automatic Trigger**: Push to `dev` branch
2. **Manual Trigger**: `workflow_dispatch` (allows manual deployment from any branch)

### Services for Backend

The pipeline includes a PostgreSQL service container for testing:

```yaml
services:
  postgres:
    image: postgres:15
    env:
      POSTGRES_USER: test
      POSTGRES_PASSWORD: test
      POSTGRES_DB: testdb
    ports:
      - 5432:5432
```

### Pipeline Steps for Backend

#### 1. Checkout Code

This step clones the repository code to the GitHub Actions runner so the pipeline can access all the source files. It uses the latest stable version of the checkout action.

```yaml
- name: Checkout code
  uses: actions/checkout@v4
```

#### 2. Setup Python Environment

This step installs Python version 3.13 on the runner and enables pip caching to speed up dependency installation in future runs.

```yaml
- name: Setup Python
  uses: actions/setup-python@v5
  with:
    python-version: "3.13"
    cache: "pip"
```

#### 3. Install Backend Dependencies

This step upgrades pip to the latest version and installs all project dependencies from the requirements.txt file, including testing libraries like pytest, pytest-cov, and pytest-asyncio.

```yaml
- name: Install dependencies
  run: |
    python -m pip install --upgrade pip
    pip install -r requirements.txt
```

#### 4. Run Database Migrations

This step sets up the test database connection and runs Alembic migrations to create the database schema. It uses the test database URL from GitHub secrets or defaults to the PostgreSQL service container.

```yaml
- name: Run database migrations for tests
  env:
    TEST_DATABASE_URL: ${{ secrets.TEST_DATABASE_URL }}
  run: |
    export TEST_DATABASE_URL=${TEST_DATABASE_URL:-postgresql://test:test@localhost:5432/testdb}
    export DATABASE_URL=$TEST_DATABASE_URL
    alembic upgrade head
```

#### 5. Run Tests with Coverage

This step executes all pytest tests with verbose output and generates code coverage reports. The coverage reports include terminal output showing missing lines and an XML report for CI/CD integration. Tests cover authentication, bills, categories, inventories, products, stores, and users.

```yaml
- name: Run tests
  env:
    TEST_DATABASE_URL: ${{ secrets.TEST_DATABASE_URL }}
  run: |
    export TEST_DATABASE_URL=${TEST_DATABASE_URL:-postgresql://test:test@localhost:5432/testdb}
    pytest --cov=app --cov-report=term-missing --cov-report=xml -v
```

#### 6. Deploy to EC2

This step connects to the EC2 instance via SSH and deploys the application. It pulls the latest code from the repository, installs dependencies, runs database migrations, and restarts the backend service using PM2.

```yaml
- name: Deploy to EC2
  uses: appleboy/ssh-action@v1.0.3
  with:
    host: ${{ secrets.EC2_BACKEND_HOST }}
    username: ${{ secrets.EC2_BACKEND_USER }}
    key: ${{ secrets.EC2_BACKEND_SSH_KEY }}
    script: |
      export NVM_DIR="$HOME/.nvm"
      [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
      cd ${{ secrets.EC2_BACKEND_APP_PATH }}
      source venv/bin/activate
      git fetch origin
      BRANCH="${{ github.ref_name }}"
      git checkout $BRANCH
      git pull origin $BRANCH
      pip install -r requirements.txt
      alembic upgrade head
      pm2 restart backend
      pm2 save
```

### Required Secrets for Backend

The backend pipeline requires the following GitHub secrets:

| Secret Name | Description |
|-------------|-------------|
| `EC2_BACKEND_HOST` | EC2 instance public IP or hostname |
| `EC2_BACKEND_USER` | SSH username (typically `ubuntu` or `ec2-user`) |
| `EC2_BACKEND_SSH_KEY` | Private SSH key for EC2 access |
| `EC2_BACKEND_APP_PATH` | Absolute path to backend application directory |

## Frontend CI/CD Pipeline

### Pipeline Configuration for Frontend

- **Workflow File**: `.github/workflows/deploy.yml`
- **Runner**: `ubuntu-latest`
- **Pipeline Name**: Frontend CI/CD Pipeline

### Triggers

1. **Automatic Trigger**: Push to `dev` branch
2. **Manual Trigger**: `workflow_dispatch` (allows manual deployment from any branch)

### Pipeline Steps

#### 1. Checkout Code

This step clones the repository code to the GitHub Actions runner so the pipeline can access all the source files. It uses the latest stable version of the checkout action.

```yaml
- name: Checkout code
  uses: actions/checkout@v4
```

#### 2. Setup Node.js Environment

This step installs Node.js version 18 on the runner and enables npm caching to speed up dependency installation in future runs.

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: "18"
    cache: "npm"
```

#### 3. Install Frontend Dependencies

This step uses npm clean install to install exact versions of dependencies from package-lock.json. This approach is faster and more reliable than regular npm install in CI environments.

```yaml
- name: Install dependencies
  run: npm ci
```

#### 4. Run Tests with Coverage

This step runs Jest tests in CI mode with coverage reporting. The CI flag optimizes the test run for continuous integration, coverage generates detailed reports, and watchAll is disabled to prevent the tests from running indefinitely. Tests cover Login, ProtectedRoute, and App initialization.

```yaml
- name: Run tests
  run: npm test -- --ci --coverage --watchAll=false
```

#### 5. Upload Coverage Report

This step uploads the test coverage reports as artifacts to GitHub Actions. The reports are always uploaded even if tests fail, and they are retained for 30 days so they can be downloaded and reviewed from the GitHub Actions UI.

```yaml
- name: Upload test coverage report
  uses: actions/upload-artifact@v4
  if: always()
  with:
    name: test-coverage-report
    path: coverage/
    retention-days: 30
```

#### 6. Deploy to EC2

This step connects to the EC2 instance via SSH and deploys the application. It pulls the latest code from the repository, installs dependencies, builds the production bundle, and restarts the frontend service using PM2.

```yaml
- name: Deploy to EC2
  uses: appleboy/ssh-action@v1.0.3
  with:
    host: ${{ secrets.EC2_FRONTEND_HOST }}
    username: ${{ secrets.EC2_FRONTEND_USER }}
    key: ${{ secrets.EC2_FRONTEND_SSH_KEY }}
    script: |
      export NVM_DIR="$HOME/.nvm"
      [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
      cd ${{ secrets.EC2_FRONTEND_APP_PATH }}
      git fetch origin
      BRANCH="${{ github.ref_name }}"
      git checkout $BRANCH
      git pull origin $BRANCH
      npm ci
      npm run build
      pm2 restart frontend
      pm2 save
```

### Required Secrets for Frontend

The frontend pipeline requires the following GitHub secrets:

| Secret Name | Description |
|-------------|-------------|
| `EC2_FRONTEND_HOST` | EC2 instance public IP or hostname |
| `EC2_FRONTEND_USER` | SSH username (typically `ubuntu` or `ec2-user`) |
| `EC2_FRONTEND_SSH_KEY` | Private SSH key for EC2 access |
| `EC2_FRONTEND_APP_PATH` | Absolute path to frontend application directory |

## Deployment Architecture

### EC2 Infrastructure

Both backend and frontend applications are deployed to AWS EC2 instances:

- **Process Manager**: PM2
- **Backend Server**: Uvicorn
- **Frontend Server**: Vite preview
