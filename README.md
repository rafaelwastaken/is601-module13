# IS601 Module 12

User authentication and calculation BREAD APIs with FastAPI, SQLAlchemy, and PostgreSQL-backed integration testing

## Project Structure

```text
app/
  crud/         # database operations
  db/           # SQLAlchemy engine/session
  factories/    # calculation factory logic
  models/       # SQLAlchemy models (user, calculation)
  schemas/      # Pydantic schemas
  main.py       # FastAPI routes (health, users, calculations)
tests/
  unit/         # schema/security/factory unit tests
  integration/  # PostgreSQL-backed API integration tests
```

## Prerequisites

- Python 3.12+
- Docker Desktop

## Local Setup

```powershell
python -m venv .venv
pip install -r requirements.txt
```

## Run the API Locally

Default database is SQLite (`app.db`) when `DATABASE_URL` is not set.

```powershell
uvicorn app.main:app --reload
```

Health check: http://127.0.0.1:8000/health

OpenAPI docs:

- Swagger UI: http://127.0.0.1:8000/docs
- ReDoc: http://127.0.0.1:8000/redoc

## Endpoints Implemented

### User

- `POST /users/register`
- `POST /users/login`

### Calculations

- `GET /calculations` (browse)
- `GET /calculations/{id}` (read)
- `POST /calculations` (add)
- `PUT /calculations/{id}` (edit)
- `DELETE /calculations/{id}` (delete)

## Run with Docker Compose

```powershell
docker compose up --build
```

Services:

- `db`: PostgreSQL 16
- `api`: FastAPI app on port `8000`

## Running Tests Locally

### Unit Tests

```powershell
pytest tests/unit
```

### Integration Tests (PostgreSQL)

Start the database:

```powershell
docker compose up -d db
```

Create the CI-style test database once:

```powershell
docker exec module12-db-1 psql -U postgres -c "CREATE DATABASE module12_test;"
```

Set the test DB URL:

```powershell
$env:TEST_DATABASE_URL="postgresql+psycopg://postgres:postgres@localhost:5432/module12_test"
```

Run integration tests:

```powershell
pytest tests/integration
```

### Full Test Suite

```powershell
pytest
```
