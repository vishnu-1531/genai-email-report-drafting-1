# Setup Guide: GenAI Email & Report Drafting System

**Project:** GenAI Email & Report Drafting System  
**Purpose:** Complete installation and configuration guide  
**For Usage Instructions:** See [Usage Guide](05_usage.md)

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Step 1: Database Setup](#step-1-database-setup)
3. [Step 2: Backend Setup](#step-2-backend-setup)
4. [Step 3: Frontend Setup](#step-3-frontend-setup)
5. [Running the Application](#running-the-application)
6. [Troubleshooting](#troubleshooting)
7. [Testing](#testing)

---

## Prerequisites

Before setting up the system, ensure you have the following installed:

- **Node.js** (v16 or higher) for frontend
- **TypeScript** (v4.5 or higher) for type checking
- **Python 3.10+** for backend
- **PostgreSQL database** - Choose one:
  - Docker or Podman with Compose (recommended for development)
  - Local PostgreSQL installation (v13+ recommended)
- **Google Gemini API key** (required for AI content generation)

---

## Step 1: Database Setup

You have two options for setting up PostgreSQL.

### Option A: Using Docker or Podman Compose (Recommended)

Docker/Podman Compose provides a consistent local development environment with minimal setup.

```powershell
# 1. Copy the environment file (from infra directory)
cp infra/.env.example infra/.env

# 2. (Optional) Edit infra/.env to customize database credentials
# Default values: POSTGRES_USER=postgres, POSTGRES_PASSWORD=postgres, POSTGRES_DB=genai_email_report

# 3. Navigate to infra directory
cd infra

# 4. Start PostgreSQL service
podman compose up -d
# or: docker compose up -d

# 5. Check service health
podman compose ps
# or: docker compose ps

# 6. View logs (optional)
podman compose logs postgres
# or: docker compose logs postgres

# The database schema (infra/database/schema.sql) will be automatically initialized
```

#### Managing the Docker/Podman Compose Environment

The database data is persisted in a volume named `genai_email_report_data`.

```powershell
# Navigate to infra directory
cd infra

# Stop services
podman compose down
# or: docker compose down

# Stop and remove data (WARNING: destroys all data in genai_email_report_data volume)
podman compose down -v
# or: docker compose down -v

# View service status
podman compose ps
# or: docker compose ps

# Access PostgreSQL CLI (optional)
podman compose exec postgres psql -U postgres -d genai_email_report
# or: docker compose exec postgres psql -U postgres -d genai_email_report
```

#### Alternative: Run from Repository Root

If you prefer running from the root directory, explicitly point to the compose file:

```powershell
# From repository root
podman compose -f infra/docker-compose.yml up -d
# or: docker compose -f infra/docker-compose.yml up -d

podman compose -f infra/docker-compose.yml down
# or: docker compose -f infra/docker-compose.yml down

podman compose -f infra/docker-compose.yml ps
# or: docker compose -f infra/docker-compose.yml ps
```

### Option B: Using Local PostgreSQL Installation

If you prefer a local PostgreSQL installation:

```sql
-- Create database
CREATE DATABASE genai_email_report;
```

Then, apply the schema using the provided SQL file to ensure your database structure matches the application requirements:

```powershell
# Using psql to apply schema (Ensure you are in the repository root)
psql -U postgres -d genai_email_report -f infra/database/schema.sql
```

_Note: While Flask-SQLAlchemy works with ORM models, applying the raw SQL schema ensures all indexes and constraints are exactly as defined in the infrastructure code._

---

## Step 2: Backend Setup

### 2.1. Installation

You have two options for setting up the Python backend environment.

#### Option A: Using `uv` (Recommended - Faster)

`uv` is a modern Python package installer that's significantly faster than pip:

```powershell
# Install uv first (one-time setup)
# Windows (PowerShell):

# We might need this to run the script
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser 

irm https://astral.sh/uv/install.ps1 | iex

$env:Path = "$env:USERPROFILE\.local\bin;$env:Path"

# Create virtual environment at root
uv venv

# Activate virtual environment
# Windows PowerShell:
.venv\Scripts\Activate.ps1
# Windows CMD:
.venv\Scripts\activate

# Navigate to backend directory
cd src/backend

# Install dependencies
uv pip install -r requirements.txt --link-mode=copy
```

#### Option B: Using Traditional pip/venv

```powershell
# Create virtual environment
python -m venv .venv

# Activate virtual environment
# Windows PowerShell:
.venv\Scripts\Activate.ps1
# Windows CMD:
.venv\Scripts\activate

# Navigate to backend directory
cd src/backend

# Install dependencies
pip install -r requirements.txt
```

### 2.2. Configuration

Create a `.env` file in the `src/backend/` directory (or copy from `src/backend/.env.example`):

```env
FLASK_APP=app.py
FLASK_ENV=development

# If using Docker/Podman Compose PostgreSQL:
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/genai_email_report

# If using local PostgreSQL installation:
# DATABASE_URL=postgresql://user:password@localhost:5432/genai_email_report

JWT_SECRET_KEY=your-secret-key-here
GEMINI_API_KEY=your-google-gemini-api-key
```

**Note:** See `src/backend/.env.example` for complete configuration options.

**Configuration Variables:**

| Variable | Description | Required |
|----------|-------------|----------|
| `DATABASE_URL` | PostgreSQL connection string | Yes |
| `JWT_SECRET_KEY` | Secret key for JWT token signing | Yes |
| `GEMINI_API_KEY` | Google Gemini API key | Yes |
| `FLASK_ENV` | Environment (development/production) | No |

**Optional Admin Bootstrap:**

To automatically create an admin user on first startup, add:

```env
ADMIN_BOOTSTRAP_ENABLED=true
ADMIN_BOOTSTRAP_USERNAME=admin
ADMIN_BOOTSTRAP_EMAIL=admin@example.com
ADMIN_BOOTSTRAP_PASSWORD=ChangeMe123!
```

---

## Step 3: Frontend Setup

### 3.1. Installation

```powershell
# Navigate to frontend directory
cd src/frontend

# Install dependencies (includes TypeScript)
npm install

# TypeScript will be included in the dependencies
# Verify TypeScript installation
npx tsc --version
```

### 3.2. Configuration

Create a `.env` file in the `src/frontend/` directory:

```env
VITE_API_BASE_URL=http://localhost:5000/api
```

---

## Running the Application

### Start Backend Server

```powershell
# Ensure virtual environment is activated. .venv exists at repository root
.venv\Scripts\Activate.ps1

# Navigate to backend directory
cd src/backend

# Start Flask server
python app.py
```

Backend will run on `http://localhost:5000`

API docs:

- Swagger UI: `http://localhost:5000/api/docs`
- OpenAPI YAML: `http://localhost:5000/api/openapi.yaml`

### Start Frontend Development Server

```powershell
# Navigate to frontend directory
cd src/frontend

# Start Vite development server
npm run dev
```

Frontend will run on `http://localhost:5173` (or similar port - check console output)

### Access the Application

1. Open your browser and navigate to the frontend URL (e.g., `http://localhost:5173`)
2. You should see the login page
3. Register a new account or login with existing credentials
4. Start using the application!

---

## Troubleshooting

### Issue: Backend Won't Start

**Symptoms:** Error when running `python app.py`

**Solutions:**

1. **Check virtual environment is activated:**

   ```powershell
   .venv\Scripts\Activate.ps1
   ```

2. **Verify database connection:**
   - If using Docker/Podman Compose: Run `podman compose ps` (or `docker compose ps`) in `infra/`
   - Check `DATABASE_URL` in `src/backend/.env`
   - Test connection: `podman compose exec postgres psql -U postgres -d genai_email_report`

3. **Check API key:**
   - Ensure `GEMINI_API_KEY` is set in `.env`

4. **Check dependencies:**
   `pip install -r requirements.txt`

### Issue: Frontend Can't Connect to Backend

**Symptoms:** API calls failing, CORS errors

**Solutions:**

1. **Verify backend is running** on `http://localhost:5000`
2. **Check `VITE_API_BASE_URL`** in frontend `.env` (Should be `http://localhost:5000/api`)
3. **Verify CORS** is enabled in Flask (default)

### Issue: Authentication Not Working

**Symptoms:** Can't login, JWT errors

**Solutions:**

1. **Check `JWT_SECRET_KEY`** in backend `.env`
2. **Clear browser localStorage** (DevTools -> Application -> Local Storage)
3. **Check database connection** (ensure users table exists)

### Issue: Gemini API Errors

**Symptoms:** Document generation fails

**Solutions:**

1. **Verify `GEMINI_API_KEY`** is correct and active
2. **Check API quota/limits** in Google Cloud Console
3. **Check backend logs** for error details

### Issue: Docker/Podman Compose PostgreSQL Issues

**Symptoms:** Container won't start, connection refused

**Solutions:**

1. **Service won't start:**

   ```powershell
   cd infra
   podman compose logs postgres
   podman compose down
   podman compose up -d
   ```

2. **Connection refused:**
   - Verify container is healthy: `podman compose ps`
   - Check port 5432 is not in use by another service

3. **Data not persisting:**
   - Avoid using `down -v` unless you want to delete data.

### Issue: TypeScript Errors

**Solutions:**

1. Reinstall: `rm -rf node_modules package-lock.json && npm install`
2. Check version: `npx tsc --version`

---

## Testing

### Backend Testing

```powershell
# Ensure virtual environment is activated. .venv exists at repository root
.venv\Scripts\Activate.ps1

# Navigate to backend directory
cd src/backend

# Run tests
python -m pytest tests/

# Run with coverage
pytest --cov=. --cov-report=html tests/
```

### Frontend Testing

```powershell
# Navigate to frontend directory
cd src/frontend

# Run tests
npm test
```

### Integration Testing

1. Start Podman/PostgreSQL
2. Start Backend
3. Start Frontend
4. Register user -> Generate Email -> Verify History

---

**Document Version:** 1.1  
**Last Updated:** January 20, 2026  
**Status:** Complete
