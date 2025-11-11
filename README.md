# FastAPI Calculator with Secure User Authentication

A production-ready FastAPI application featuring a calculator API with secure user authentication, built with SQLAlchemy, Pydantic, and PostgreSQL. The project includes comprehensive testing, CI/CD automation, and Docker containerization.
Link to Docker Hub: https://hub.docker.com/repository/docker/cerechukwu27/cerechukwu27_mod10/general

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [Running Tests](#running-tests)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [CI/CD Pipeline](#cicd-pipeline)
- [Docker Hub](#docker-hub)
- [Security Considerations](#security-considerations)

## Overview

This project is a FastAPI-based web application that provides both calculator functionality and secure user management. It demonstrates modern web development practices including:

- RESTful API design
- Database modeling with SQLAlchemy ORM
- Data validation with Pydantic v2
- Password hashing with bcrypt
- JWT token-based authentication
- Comprehensive test coverage (unit, integration, and end-to-end)
- Automated CI/CD with GitHub Actions
- Containerization with Docker

## Features

### Calculator API
- Addition, subtraction, multiplication, and division operations
- Input validation and error handling
- Interactive web interface

### User Authentication System
- Secure user registration with password hashing
- JWT token-based authentication
- Email and username uniqueness validation
- Password complexity requirements
- User session management

### Security Features
- Bcrypt password hashing
- JWT token generation and verification
- Protected API endpoints
- SQL injection prevention through ORM
- Input sanitization

## Technology Stack

- **Framework**: FastAPI 0.115.4
- **Database**: PostgreSQL with SQLAlchemy 2.0.36
- **Authentication**: JWT (python-jose 3.3.0), passlib 1.7.4, bcrypt 4.2.1
- **Validation**: Pydantic 2.9.2
- **Testing**: pytest 8.3.3, pytest-cov 6.0.0, Playwright 1.48.0
- **Containerization**: Docker & Docker Compose
- **CI/CD**: GitHub Actions
- **Server**: Uvicorn 0.32.0
- **Database Admin**: pgAdmin 4

## Project Structure

```
.
├── .github/
│   └── workflows/
│       ├── python-app.yml          # CI/CD pipeline configuration
│       └── docker-publish.yml      # Docker Hub deployment
├── app/
│   ├── auth/
│   │   ├── __init__.py
│   │   └── dependencies.py         # Authentication dependencies
│   ├── models/
│   │   ├── __init__.py
│   │   └── user.py                 # SQLAlchemy User model
│   ├── operations/
│   │   └── __init__.py             # Calculator operations
│   ├── schemas/
│   │   ├── __init__.py
│   │   ├── base.py                 # Base Pydantic schemas
│   │   └── user.py                 # User-related schemas
│   ├── __init__.py
│   ├── config.py                   # Application configuration
│   ├── database.py                 # Database connection setup
│   └── database_init.py            # Database initialization
├── tests/
│   ├── e2e/
│   │   ├── __init__.py
│   │   └── test_e2e.py             # End-to-end tests
│   ├── integration/
│   │   ├── __init__.py
│   │   ├── test_database.py        # Database integration tests
│   │   ├── test_dependencies.py    # Auth dependency tests
│   │   ├── test_fastapi_calculator.py
│   │   ├── test_schema_base.py     # Schema validation tests
│   │   ├── test_user.py            # User model tests
│   │   └── test_user_auth.py       # Authentication tests
│   ├── unit/
│   │   ├── __init__.py
│   │   └── test_calculator.py      # Unit tests for calculator
│   ├── __init__.py
│   └── conftest.py                 # Pytest configuration & fixtures
├── templates/
│   └── index.html                  # Web interface
├── .gitignore
├── Dockerfile                      # Production container configuration
├── docker-compose.yml              # Multi-container orchestration
├── main.py                         # Application entry point
├── pytest.ini                      # Pytest configuration
├── requirements.txt                # Python dependencies
└── README.md                       # This file
```

## Prerequisites

- Python 3.10 or higher
- PostgreSQL 12 or higher
- Docker and Docker Compose (optional, for containerized deployment)
- Git

## Installation

### Local Development Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/fastapi-calculator.git
   cd fastapi-calculator
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Install Playwright browsers** (for end-to-end tests)
   ```bash
   playwright install
   ```

5. **Set up environment variables**
   
   Create a `.env` file in the root directory:
   ```env
   DATABASE_URL=postgresql://postgres:postgres@localhost:5432/fastapi_db
   SECRET_KEY=your-secret-key-change-this-in-production
   ```

### Docker Setup

1. **Start all services**
   ```bash
   docker-compose up -d
   ```

   This will start:
   - FastAPI application (port 8000)
   - PostgreSQL database (port 5432)
   - pgAdmin (port 5050)

2. **Initialize the database**
   ```bash
   docker-compose exec web python app/database_init.py
   ```

## Configuration

### Database Configuration

The application uses PostgreSQL as its database. Configure the connection in `app/config.py` or via environment variables:

```python
DATABASE_URL=postgresql://user:password@host:port/database_name
```

### Security Configuration

**Important**: Change the following default values in production:

- `SECRET_KEY` in `app/models/user.py` (move to environment variables)
- `ALGORITHM` for JWT (default: HS256)
- `ACCESS_TOKEN_EXPIRE_MINUTES` (default: 30)

## Running the Application

### Local Development

```bash
# Activate virtual environment
source venv/bin/activate

# Initialize database
python app/database_init.py

# Run the application
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### Using Docker Compose

```bash
# Start services
docker-compose up -d

# View logs
docker-compose logs -f web

# Stop services
docker-compose down
```

### Access Points

- **Application**: http://localhost:8000
- **API Documentation**: http://localhost:8000/docs
- **Alternative API Docs**: http://localhost:8000/redoc
- **pgAdmin**: http://localhost:5050 (admin@example.com / admin)

## Running Tests

The project includes comprehensive test coverage across three levels:

### Run All Tests

```bash
# Activate virtual environment
source venv/bin/activate

# Run all tests with coverage
pytest

# Run with verbose output
pytest -v -s
```

### Run Specific Test Categories

```bash
# Unit tests only
pytest tests/unit/

# Integration tests only
pytest tests/integration/

# End-to-end tests only
pytest tests/e2e/

# Run tests with specific markers
pytest -m e2e
```

### Test Coverage Report

```bash
# Generate coverage report
pytest --cov=app --cov-report=html

# View coverage report
open htmlcov/index.html  # On macOS
# Or navigate to htmlcov/index.html in your browser
```

### Test Database Management

The test suite uses a dedicated test database configured in `tests/conftest.py`:

```bash
# Preserve database after tests (for debugging)
pytest --preserve-db

# Run slow tests (normally skipped)
pytest --run-slow
```

### Testing with Docker

```bash
# Run tests inside Docker container
docker-compose exec web pytest

# Run with coverage
docker-compose exec web pytest --cov=app --cov-report=term-missing
```

## API Documentation

### Calculator Endpoints

#### POST /add
Add two numbers.

**Request Body:**
```json
{
  "a": 10,
  "b": 5
}
```

**Response:**
```json
{
  "result": 15
}
```

#### POST /subtract
Subtract two numbers.

#### POST /multiply
Multiply two numbers.

#### POST /divide
Divide two numbers.

**Error Response (Division by Zero):**
```json
{
  "error": "Cannot divide by zero!"
}
```

### User Authentication Endpoints

*Note: Authentication endpoints are defined in the User model and can be extended to FastAPI routes.*

#### User Registration
Create a new user account with hashed password storage.

**Required Fields:**
- `first_name`: String (max 50 characters)
- `last_name`: String (max 50 characters)
- `email`: Valid email address (unique)
- `username`: String (3-50 characters, unique)
- `password`: String (min 6 characters, must contain uppercase, lowercase, and digit)

#### User Authentication
Authenticate user and receive JWT access token.

**Returns:**
- `access_token`: JWT token string
- `token_type`: "bearer"
- `user`: User object (without password)

## Database Schema

### Users Table

| Column | Type | Constraints | Description |
|--------|------|-------------|-------------|
| id | UUID | PRIMARY KEY | Unique user identifier |
| first_name | VARCHAR(50) | NOT NULL | User's first name |
| last_name | VARCHAR(50) | NOT NULL | User's last name |
| email | VARCHAR(120) | UNIQUE, NOT NULL | User's email address |
| username | VARCHAR(50) | UNIQUE, NOT NULL | User's username |
| password | VARCHAR(255) | NOT NULL | Bcrypt hashed password |
| is_active | BOOLEAN | NOT NULL, DEFAULT TRUE | Account active status |
| is_verified | BOOLEAN | NOT NULL, DEFAULT FALSE | Email verification status |
| last_login | TIMESTAMP | NULLABLE | Last login timestamp |
| created_at | TIMESTAMP | NOT NULL | Account creation timestamp |
| updated_at | TIMESTAMP | NOT NULL | Last update timestamp |

### Pydantic Schemas

#### UserBase
Base schema with common user fields:
- `first_name`, `last_name`, `email`, `username`

#### PasswordMixin
Password validation with requirements:
- Minimum 6 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one digit

#### UserCreate
Combines UserBase and PasswordMixin for user registration.

#### UserResponse
Response schema excluding password field, includes:
- All UserBase fields
- `id`, `is_active`, `is_verified`
- `created_at`, `updated_at`

#### Token
Authentication response schema:
- `access_token`: JWT token
- `token_type`: "bearer"
- `user`: UserResponse object

## CI/CD Pipeline

The project uses GitHub Actions for continuous integration and deployment.

### Workflow: CI/CD Testing

**File**: `.github/workflows/python-app.yml`

**Triggers**:
- Push to `main` branch
- Pull requests to `main` branch

**Steps**:
1. Checkout code
2. Set up Python 3.13
3. Cache dependencies
4. Start PostgreSQL service
5. Install dependencies
6. Run unit tests with coverage
7. Run integration tests
8. Run end-to-end tests

**PostgreSQL Service Configuration**:
- Image: `postgres:latest`
- Database: `mytestdb`
- User: `user`
- Password: `password`
- Port: `5432`
- Health checks enabled

### Workflow: Docker Hub Deployment

**File**: `.github/workflows/docker-publish.yml`

**Triggers**:
- Push to `main` branch
- Version tags (`v*`)
- Pull requests to `main` branch

**Steps**:
1. Checkout code
2. Set up Docker Buildx
3. Login to Docker Hub
4. Extract metadata (tags, labels)
5. Build and push Docker image

**Required Secrets**: (Not added here but will be in the future)
- `DOCKER_USERNAME`: Your Docker Hub username
- `DOCKER_PASSWORD`: Your Docker Hub access token

### Setting Up GitHub Secrets

1. Navigate to your GitHub repository
2. Go to **Settings** > **Secrets and variables** > **Actions**
3. Click **New repository secret**
4. Add the following secrets:
   - `DOCKER_USERNAME`
   - `DOCKER_PASSWORD`

### Viewing Workflow Runs

1. Go to the **Actions** tab in your GitHub repository
2. Select a workflow run to view details
3. Check logs for each step
4. View test results and coverage reports

## Docker Hub

### Repository Information

**Docker Hub Repository**: [yourusername/fastapi-calculator](https://hub.docker.com/r/yourusername/fastapi-calculator)

### Pulling the Image

```bash
docker pull yourusername/fastapi-calculator:latest
```

### Running the Container

```bash
# Run with environment variables
docker run -d \
  -p 8000:8000 \
  -e DATABASE_URL=postgresql://user:password@host:5432/database \
  --name fastapi-calculator \
  yourusername/fastapi-calculator:latest
```

### Available Tags

- `latest`: Most recent build from main branch
- `v1.0.0`, `v1.0`, `v1`: Semantic version tags
- `main`: Latest build from main branch
- `sha-<commit>`: Specific commit builds

### Building Locally

```bash
# Build the image
docker build -t fastapi-calculator .

# Tag for Docker Hub
docker tag fastapi-calculator yourusername/fastapi-calculator:latest

# Push to Docker Hub
docker push yourusername/fastapi-calculator:latest
```

## Security Considerations

### Password Security

- Passwords are hashed using bcrypt with automatic salt generation
- Plain-text passwords are never stored in the database
- Password complexity requirements enforced via Pydantic validation

### Authentication Security

- JWT tokens with configurable expiration
- Bearer token authentication scheme
- Token verification on protected endpoints
- User session tracking with `last_login` timestamp

### Database Security

- SQL injection prevention through SQLAlchemy ORM
- Prepared statements for all queries
- Unique constraints on email and username
- Input validation at the Pydantic schema level

### Best Practices Implemented

1. **Environment Variables**: Sensitive data stored in environment variables
2. **Dependency Injection**: FastAPI's dependency system for authentication
3. **Error Handling**: Comprehensive exception handling with appropriate HTTP status codes
4. **Input Validation**: Pydantic schemas validate all incoming data
5. **Docker Security**: Non-root user in Docker container
6. **Health Checks**: Container health monitoring
7. **HTTPS Ready**: Application ready for reverse proxy with SSL/TLS

### Production Recommendations

1. **Change Default Secrets**: Update `SECRET_KEY` and use strong, random values
2. **Use HTTPS**: Deploy behind a reverse proxy (nginx, Traefik) with SSL/TLS
3. **Rate Limiting**: Implement rate limiting for authentication endpoints
4. **Monitoring**: Add application monitoring and logging
5. **Database Backups**: Implement regular database backup strategy
6. **Token Refresh**: Implement refresh token mechanism for long-lived sessions
7. **Email Verification**: Enable email verification before account activation
8. **Two-Factor Authentication**: Consider adding 2FA for enhanced security

## Testing Strategy

### Test Pyramid

The project follows the testing pyramid approach:

1. **Unit Tests** (70%): Fast, isolated tests for individual functions
   - Calculator operations
   - Password hashing and verification
   - Schema validation
   - Token generation and verification

2. **Integration Tests** (20%): Tests with database interactions
   - User registration and authentication
   - Database constraints and uniqueness
   - Session handling and transactions
   - User model operations

3. **End-to-End Tests** (10%): Full application flow tests
   - Web interface functionality
   - API endpoint integration
   - User workflows

### Test Fixtures

Defined in `tests/conftest.py`:

- `db_session`: Database session for each test
- `test_user`: Pre-created test user
- `fake_user_data`: Generated fake user data
- `seed_users`: Multiple test users for bulk operations
- `fastapi_server`: Running FastAPI server for E2E tests
- `browser_context`: Playwright browser for UI tests
- `page`: Browser page for individual tests

### Test Database Isolation

- Each test runs in a clean database state
- Tables are truncated after each test
- Transaction rollback for failed tests
- Option to preserve database for debugging (`--preserve-db`)

## Development Workflow

### Branch Strategy

```bash
# Create feature branch
git checkout -b feature/your-feature-name

# Make changes and commit
git add .
git commit -m "Add feature description"

# Push to GitHub
git push origin feature/your-feature-name

# Create pull request on GitHub
```

### Code Quality

```bash
# Run linting
pylint app/ tests/

# Format code
black app/ tests/

# Sort imports
isort app/ tests/

# Type checking
mypy app/
```

### Pre-commit Checks

Before committing, ensure:

1. All tests pass: `pytest`
2. Code coverage is maintained: `pytest --cov=app`
3. No linting errors: `pylint app/`
4. Code is formatted: `black app/ tests/`

## Troubleshooting

### Common Issues

#### Database Connection Errors

```bash
# Check PostgreSQL is running
docker-compose ps

# Restart PostgreSQL
docker-compose restart db

# Check logs
docker-compose logs db
```

#### Port Already in Use

```bash
# Find process using port 8000
lsof -i :8000

# Kill the process
kill -9 <PID>
```

#### Test Failures

```bash
# Run tests with verbose output
pytest -v -s

# Run specific test file
pytest tests/unit/test_calculator.py -v

# Debug with breakpoints
pytest --pdb
```

#### Docker Build Issues

```bash
# Clean Docker cache
docker system prune -a

# Rebuild without cache
docker-compose build --no-cache
```


