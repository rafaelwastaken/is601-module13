# IS601 Module 13

JWT login/registration with FastAPI, client-side form validation, Playwright end-to-end tests, and Docker-based CI/CD.

## Project Structure

```text
app/
  crud/         # database operations
  db/           # SQLAlchemy engine/session
  factories/    # calculation factory logic
  models/       # SQLAlchemy models (user, calculation)
  schemas/      # Pydantic schemas
  static/       # login/register frontend pages and assets
  main.py       # FastAPI routes (health, auth, users, calculations)
tests/
  unit/         # unit tests
  integration/  # API integration tests
  e2e/          # Playwright browser tests
```

## Prerequisites

- Python 3.12+
- Node.js 20+
- Docker Desktop

## Local Setup

### Python dependencies

```powershell
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
```

### Node dependencies (Playwright)

```powershell
npm install
npx playwright install chromium
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

## Front-End Pages

- Registration page: http://127.0.0.1:8000/register.html
- Login page: http://127.0.0.1:8000/login.html

Client-side behavior:

- Email format validation
- Minimum password length validation (8)
- Confirm password validation on register page
- JWT token stored in `localStorage` key `jwt_token` on success

## API Endpoints

### JWT Auth (Module 13)

- `POST /register` with `email` and `password`
- `POST /login` with `email` and `password`

Successful responses return:

- `access_token`
- `token_type` (`bearer`)

### Existing User Routes

- `POST /users/register`
- `POST /users/login`

### Calculations

- `GET /calculations`
- `GET /calculations/{id}`
- `POST /calculations`
- `PUT /calculations/{id}`
- `DELETE /calculations/{id}`

## Running Tests Locally

### Unit Tests

```powershell
pytest tests/unit
```

### Integration Tests

```powershell
pytest tests/integration
```

### Playwright E2E Tests

Start the API in one terminal:

```powershell
uvicorn app.main:app --host 127.0.0.1 --port 8000
```

Run E2E in another terminal:

```powershell
npm run test:e2e
```

## Run with Docker Compose

```powershell
docker compose up --build
```

Services:

- `db`: PostgreSQL 16
- `api`: FastAPI app on port `8000`
