# Repository Structure

**Project:** GenAI Email & Report Drafting System  
**Last Updated:** 2026-01-09  
**Status:** Complete Implementation

---

## Overview

This document provides a comprehensive overview of the repository structure for the GenAI Email & Report Drafting System. The project follows an N-Tier architecture with clear separation between frontend, backend, database, and documentation layers.

---

## Complete Directory Structure

```text
genai-email-report-drafting/
├── .github/                    # GitHub configuration
│   ├── workflows/              # CI/CD workflows
│   ├── ISSUE_TEMPLATE/         # Issue templates
│   ├── prompts/                # Prompt engineering guides
│   ├── workitems/              # Phase work items
│   └── copilot-instructions.md # GitHub Copilot instructions
│
├── .cursor/                    # Cursor IDE configuration
│   └── rules/                  # Cursor rules (.mdc files)
│       ├── 01_educational-content-rules.mdc
│       ├── 02_repository-structure.mdc
│       ├── 03_quality-assurance.mdc
│       ├── 04_markdown-standards.mdc
│       ├── 05_primary-directives.mdc
│       ├── 06_cross-domain-integration.mdc
│       └── 07_file-naming-conventions.mdc
│
├── frontend/                   # React.js with TypeScript (Presentation Layer)
│   ├── src/
│   │   ├── api/                # API client
│   │   │   └── client.ts       # TypeScript API client with JWT handling
│   │   ├── components/         # Reusable React components
│   │   │   ├── AdminRoute.tsx  # Admin route protection
│   │   │   ├── Layout.tsx      # Main layout component
│   │   │   └── PrivateRoute.tsx # Private route protection
│   │   ├── pages/              # Page components
│   │   │   ├── AdminDashboard.tsx
│   │   │   ├── EmailGenerator.tsx
│   │   │   ├── History.tsx
│   │   │   ├── Login.tsx
│   │   │   ├── Register.tsx
│   │   │   └── ReportGenerator.tsx
│   │   ├── store/              # Redux state management
│   │   │   ├── authSlice.ts    # Authentication state
│   │   │   ├── documentsSlice.ts # Documents state
│   │   │   ├── hooks.ts        # Typed Redux hooks
│   │   │   └── index.ts        # Store configuration
│   │   ├── types/              # TypeScript type definitions
│   │   │   └── index.ts        # Shared types and interfaces
│   │   ├── App.tsx             # Main application component
│   │   ├── main.tsx            # Application entry point
│   │   └── index.css           # Global styles
│   ├── public/                 # Static assets
│   │   └── vite.svg
│   ├── dist/                   # Build output (generated)
│   ├── eslint.config.js        # ESLint configuration
│   ├── index.html              # HTML entry point
│   ├── package.json            # Node.js dependencies
│   ├── package-lock.json       # Dependency lock file
│   ├── postcss.config.js       # PostCSS configuration
│   ├── tailwind.config.js      # Tailwind CSS configuration
│   ├── tsconfig.json           # TypeScript configuration
│   ├── tsconfig.app.json       # App-specific TypeScript config
│   ├── tsconfig.node.json      # Node-specific TypeScript config
│   ├── vite.config.ts          # Vite build configuration
│   └── README.md               # Frontend documentation
│
├── backend/                    # Flask REST API (Application Layer)
│   ├── models/                 # SQLAlchemy database models
│   │   ├── __init__.py
│   │   ├── user.py             # User authentication model
│   │   ├── document.py         # Document storage model
│   │   └── audit_log.py        # Audit logging model
│   ├── routes/                 # API endpoint handlers
│   │   ├── __init__.py
│   │   ├── auth.py             # Authentication endpoints
│   │   ├── documents.py        # Document generation endpoints
│   │   ├── history.py          # History retrieval endpoints
│   │   ├── admin.py            # Admin endpoints
│   │   └── README.md           # Routes documentation
│   ├── services/               # Business logic layer
│   │   ├── __init__.py
│   │   ├── gemini_service.py   # Google Gemini API integration
│   │   ├── prompt_engine.py    # Prompt construction logic
│   │   └── README.md           # Services documentation
│   ├── tests/                  # Test suite
│   │   ├── __init__.py
│   │   ├── conftest.py         # Pytest configuration
│   │   ├── smoke_test.py       # Smoke test script
│   │   ├── test_admin.py       # Admin endpoint tests
│   │   ├── test_auth.py        # Authentication tests
│   │   ├── test_documents.py   # Document generation tests
│   │   ├── test_gemini_service.py # Gemini service tests
│   │   ├── test_history.py    # History endpoint tests
│   │   ├── test_models.py      # Database model tests
│   │   └── test_prompt_engine.py # Prompt engine tests
│   ├── app.py                  # Flask application factory
│   ├── config.py               # Configuration management
│   ├── db.py                   # Database initialization
│   ├── demo_gemini_integration.py # Demo script
│   ├── manual_test.ps1         # PowerShell manual test script
│   ├── manual_test.sh          # Bash manual test script
│   ├── pyproject.toml          # Python project configuration
│   ├── requirements.txt        # Python dependencies
│   ├── README.md               # Backend documentation
│   └── validation_results.md   # Validation results
│
├── database/                   # Database schema (Data Layer)
│   ├── schema.sql              # PostgreSQL schema definition
│   └── README.md                # Database setup instructions
│
├── docs/                       # Project documentation
│   ├── 01_abstract.md          # Project abstract
│   ├── 02_requirements.md      # Requirements specification
│   ├── 03_usage.md              # Usage guide
│   ├── 04_technical.md         # Technical documentation
│   ├── 05_architecture_plan.md # Architecture planning
│   ├── 06_comparison.md        # Approach comparison
│   ├── 07_repository_structure.md # This file
│   ├── diagrams/               # Architecture diagrams
│   │   ├── system-architecture.mmd
│   │   ├── system-architecture-simple.mmd
│   │   ├── system-architecture-basic.mmd
│   │   ├── system-architecture-requirements.mmd
│   │   └── README.md
│   ├── images/                 # Documentation images
│   ├── masterplan.md           # Master plan document
│   └── STRUCTURE_ANALYSIS.md   # Structure analysis
│
├── tools/                      # Utility scripts and automation
│   └── psscripts/              # PowerShell scripts
│       ├── Compare-DocFiles.ps1
│       ├── Find-DuplicateContent.ps1
│       ├── Get-FileStats.ps1
│       ├── Get-MarkdownSummary.ps1
│       ├── Get-RepoStats.ps1
│       ├── Quick-HealthCheck.ps1
│       ├── RepoConfig.psd1
│       ├── Run-MarkdownLintAndLychee.ps1
│       ├── Test-ContentCompliance.ps1
│       ├── Validate-FileReferences.ps1
│       ├── Verify-ZeroCopy.ps1
│       ├── Run-AllPSScripts.ps1
│       └── README.md
│
├── .python-version             # Python version specification (3.12.10)
├── CODE_OF_CONDUCT.md          # Code of conduct
├── CONTRIBUTING.md             # Contribution guidelines
├── LICENSE                     # MIT License
├── lychee.toml                 # Lychee link checker configuration
├── pyproject.toml              # Root Python project configuration
└── README.md                   # Main project README
```

---

## Layer Organization

### Presentation Layer (`frontend/`)

**Technology:** React.js with TypeScript, Redux Toolkit, Tailwind CSS, Vite

**Key Directories:**

- `src/api/` - API client with JWT token handling
- `src/components/` - Reusable UI components (Layout, Route guards)
- `src/pages/` - Page-level components (Login, Register, Generators, History, Admin)
- `src/store/` - Redux state management (auth, documents)
- `src/types/` - TypeScript type definitions

**Key Files:**

- `src/App.tsx` - Main application with routing
- `src/main.tsx` - Application entry point
- `vite.config.ts` - Build configuration
- `tailwind.config.js` - CSS framework configuration

---

### Application Layer (`backend/`)

**Technology:** Flask, SQLAlchemy, Flask-JWT-Extended, Google Generative AI

**Key Directories:**

- `models/` - Database models (User, Document, AuditLog)
- `routes/` - API endpoints (auth, documents, history, admin)
- `services/` - Business logic (Gemini service, prompt engine)
- `tests/` - Comprehensive test suite (127+ tests)

**Key Files:**

- `app.py` - Flask application factory
- `config.py` - Environment-based configuration
- `db.py` - Database initialization
- `requirements.txt` - Python dependencies

---

### Data Layer (`database/`)

**Technology:** PostgreSQL, SQLAlchemy ORM

**Key Files:**

- `schema.sql` - Complete database schema
- `README.md` - Database setup instructions

**Tables:**

- `users` - User accounts and authentication
- `documents` - Generated emails and reports
- `audit_logs` - System audit trail

---

### Documentation Layer (`docs/`)

**Contents:**

- Project documentation (abstract, requirements, usage, technical)
- Architecture diagrams (Mermaid source files)
- Master plan and structure analysis

---

## Key Files Reference

### Configuration Files

- `.python-version` - Python version (3.12.10)
- `pyproject.toml` - Root Python project config
- `backend/pyproject.toml` - Backend-specific config
- `frontend/package.json` - Frontend dependencies
- `backend/requirements.txt` - Backend dependencies
- `lychee.toml` - Link checker configuration

### Entry Points

- `frontend/src/main.tsx` - Frontend entry point
- `backend/app.py` - Backend entry point
- `database/schema.sql` - Database initialization

### Documentation

- `README.md` - Main project documentation
- `docs/07_repository_structure.md` - This file (repository structure)
- `docs/02_requirements.md` - Requirements specification
- `docs/05_architecture_plan.md` - Architecture planning
- `CONTRIBUTING.md` - Contribution guidelines
- `SECURITY.md` - Security policy

---

## N-Tier Architecture Mapping

| Layer | Directory | Technology |
|-------|-----------|------------|
| **Presentation** | `frontend/` | React.js + TypeScript |
| **Application** | `backend/` | Flask REST API |
| **Data** | `database/` | PostgreSQL |
| **AI Service** | `backend/services/` | Google Gemini API |

---

## File Naming Conventions

### Python Files (Backend)
- Snake_case: `gemini_service.py`, `prompt_engine.py`
- Descriptive names: `document_service.py`, `auth_routes.py`

### TypeScript/React Files (Frontend)
- **Components**: PascalCase: `EmailGenerator.tsx`, `Login.tsx`
- **API Client**: camelCase: `client.ts`
- **Types**: PascalCase: `index.ts` (with interfaces/types)

### Documentation Files
- Numbered sequence: `01_abstract.md`, `02_requirements.md`
- Descriptive names: `masterplan.md`, `STRUCTURE_ANALYSIS.md`

---

## Development Workflow

### Adding New Features

1. **Frontend Feature:**
   - Add component in `frontend/src/components/` or `frontend/src/pages/`
   - Update types in `frontend/src/types/`
   - Update Redux store if needed in `frontend/src/store/`

2. **Backend Feature:**
   - Add route in `backend/routes/`
   - Add service logic in `backend/services/`
   - Update models in `backend/models/` if needed
   - Add tests in `backend/tests/`

3. **Database Changes:**
   - Update `database/schema.sql`
   - Create migration if using Flask-Migrate

---

## Testing Structure

### Backend Tests (`backend/tests/`)

- `test_models.py` - Database model tests
- `test_auth.py` - Authentication tests
- `test_documents.py` - Document generation tests
- `test_admin.py` - Admin endpoint tests
- `test_gemini_service.py` - Gemini API integration tests
- `test_prompt_engine.py` - Prompt construction tests
- `test_history.py` - History retrieval tests
- `conftest.py` - Pytest configuration and fixtures

**Run tests:**
```powershell
cd backend
pytest tests/
```

### Frontend Tests

TypeScript compilation and ESLint validation ensure type safety and code quality.

---

## Build and Deployment

### Frontend Build

```powershell
cd frontend
npm run build  # Outputs to frontend/dist/
```

### Backend Deployment

- Flask application runs from `backend/app.py`
- Uses environment variables for configuration
- Database migrations via Flask-Migrate (if configured)

---

## Related Documentation

- [Architecture Plan](05_architecture_plan.md) - Detailed architecture documentation
- [Requirements Specification](02_requirements.md) - System requirements
- [Usage Guide](03_usage.md) - Setup and usage instructions
- [Technical Documentation](04_technical.md) - Technical implementation details
- [Structure Analysis](STRUCTURE_ANALYSIS.md) - Folder structure analysis

---

**Last Updated:** 2026-01-09  
**Maintained By:** Viswanatha Swamy P K
