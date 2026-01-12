# GenAI Email & Report Drafting System

[![License](https://img.shields.io/badge/License-MIT-purple)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Complete-success)](https://github.com/SrivariHSSPL-2026/genai-email-report-drafting)
[![Architecture](https://img.shields.io/badge/Architecture-N--Tier-FF6B35)](docs/05_architecture_plan.md)
[![Python](https://img.shields.io/badge/Python-3.12.10-ABCDEF?logo=python&logoColor=white)](https://www.python.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-pink?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)](https://react.dev/)
[![Flask](https://img.shields.io/badge/Flask-3.x-green)](https://flask.palletsprojects.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-13%2B-brown?logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-API-yellow?logo=googlegemini&logoColor=white)](https://ai.google.dev/)
[![Maintainer](https://img.shields.io/badge/Maintainer-Viswanatha%20Swamy%20P%20K-blue)](https://github.com/SrivariHSSPL-2026)

An AI-driven application that generates professional emails and reports using **Google Gemini**, built with an **N-Tier enterprise architecture** comprising **React.js with TypeScript**, **Flask**, and **PostgreSQL**, with **JWT-based authentication and role-based access control**.

---

## Abstract

Professional communication through emails and reports is a time-consuming and repetitive activity in corporate and academic environments. This project presents an **AI-Driven Email and Report Drafting System** that leverages **Generative Artificial Intelligence (GenAI)** using **Google Gemini Large Language Models** to automatically generate high-quality, context-aware written content.

The system is implemented using an **N-Tier architecture** comprising a **React.js with TypeScript frontend**, a **Flask-based RESTful backend**, and a **PostgreSQL database**. Secure access control is enforced using **JWT-based authentication** with **Role-Based Access Control (RBAC)**. The solution demonstrates how Generative AI can be integrated into enterprise-grade software systems while maintaining controlled complexity suitable for academic evaluation.

The system employs **instruction-based prompt engineering** optimized for Google Gemini's instruction-following capabilities. Users can generate professional emails and reports by providing purpose, tone, and contextual inputs. All generated content is persisted in the database for future reference and auditing purposes.

The project successfully demonstrates the practical application of Generative AI within a secure, enterprise-style architecture. By balancing simplicity and architectural rigor, the system provides a strong academic foundation while remaining extensible for real-world deployment scenarios.

---

## Problem Statement

Effective written communication plays a critical role in professional workflows. However, several challenges exist in the current approach to drafting professional emails and reports:

- **Time Consumption**: Manual drafting of emails and reports consumes significant time that could be better utilized for other tasks
- **Consistency Challenges**: Maintaining consistent tone and structure across multiple documents is challenging, especially when working under time constraints
- **Security Gaps**: Many AI-powered tools lack proper secure access control and data persistence, making them unsuitable for enterprise environments
- **Architectural Limitations**: Most small-scale AI tools ignore enterprise architectural principles, making them difficult to scale and maintain
- **Quality Variability**: Output quality often depends heavily on the writer's skill level and available time, leading to inconsistent results

This project addresses these challenges by providing a secure, enterprise-grade solution that automates content generation while maintaining professional standards and architectural best practices.

---

## 📌 Overview

Writing professional emails and reports is a repetitive and time-consuming task in corporate and academic environments. This project leverages **Generative AI (GenAI)** to automate content drafting while maintaining clarity, tone, and structure.

The system allows authenticated users to generate emails and reports based on purpose, tone, and contextual inputs, while persisting generated content for future reference and auditing.

---

## 🎯 Key Features

- ✨ **AI-Powered Content Generation**
  - Email generation with customizable tone (professional, formal, casual, friendly)
  - Report generation with multiple structures (executive summary, detailed, bullet points)
  - Context-aware content using Google Gemini Large Language Models
  - Real-time generation with proper error handling and retry logic

- 🧱 **Enterprise-Grade N-Tier Architecture**
  - **Presentation Layer**: React.js with TypeScript, Redux Toolkit, and Tailwind CSS
  - **Application Layer**: Flask REST API with JWT authentication and RBAC
  - **Data Layer**: PostgreSQL with SQLAlchemy ORM and proper relationships
  - **AI Service Layer**: Google Gemini API integration with prompt engineering
  - Clear separation of concerns enabling scalability and maintainability

- 🔐 **Secure Authentication & Authorization**
  - JWT-based stateless authentication with token refresh support
  - Secure password hashing using Werkzeug security utilities
  - Protected API endpoints with role-based access control
  - Login/Register pages with proper validation and error handling

- 👥 **Role-Based Access Control (RBAC)**
  - **User Role**: Generate documents, view personal history
  - **Admin Role**: Full system access including audit logs and system metrics
  - Admin dashboard with system summary, audit log viewer, and user management
  - Route protection at both frontend (PrivateRoute, AdminRoute) and backend levels

- 🗄️ **Comprehensive Data Persistence**
  - User accounts with secure credential storage
  - Document history with metadata (tone, structure, timestamps)
  - Complete audit trail for all system actions (login, document generation, admin operations)
  - Foreign key relationships with cascade delete for data integrity

- 🧠 **Advanced Prompt Engineering**
  - Structured prompt construction optimized for Google Gemini
  - Tone selection: Professional, Formal, Casual, Friendly
  - Report structures: Executive Summary, Detailed, Bullet Points
  - Instruction-based prompts with role definition and format constraints
  - Input validation and sanitization before API calls

- 📜 **Document History & Management**
  - User-specific document history with filtering (all, email, report)
  - Document detail view with full content and metadata
  - Chronological sorting and pagination support
  - Cross-user access prevention for security

- 📊 **Admin Dashboard & Audit System**
  - System summary with user counts, document statistics, and recent activity
  - Comprehensive audit log viewer with pagination
  - Real-time metrics and activity monitoring
  - Request correlation IDs for traceability across system layers

- 🧪 **Comprehensive Testing Suite**
  - 127+ unit and integration tests covering all major components
  - Backend tests with mocked Gemini API (no external dependencies)
  - Frontend TypeScript compilation and ESLint validation
  - Test coverage for authentication, RBAC, document generation, and admin features

- 🚀 **Production-Ready Features**
  - Error handling with user-friendly messages
  - Request correlation tracking for debugging
  - API rate limiting and timeout management
  - Environment-based configuration (development, production, test)
  - CI/CD workflows for automated testing and validation

---

## 🏗️ System Architecture

```mermaid
---
config:
  layout: elk
---
flowchart TB
    Frontend["React.js with TypeScript<br/>Presentation Layer"] -->|REST API| Backend["Flask REST API<br/>Application Layer"]
    Backend -->|ORM/SQL| Database[("PostgreSQL<br/>Data Layer")]
    Backend -->|API Calls| Gemini["Google Gemini API<br/>AI Service Layer"]
    Gemini -->|Generated Content| Backend
    Backend -->|Response| Frontend
    
    subgraph "Frontend Components"
        Login["Login/Register"]
        EmailGen["Email Generator"]
        ReportGen["Report Generator"]
        History["Document History"]
        Admin["Admin Dashboard"]
    end
    
    subgraph "Backend Services"
        Auth["Authentication<br/>JWT + RBAC"]
        Docs["Document Generation"]
        Prompt["Prompt Engine"]
        AdminAPI["Admin API"]
    end
    
    Frontend -.-> Login
    Frontend -.-> EmailGen
    Frontend -.-> ReportGen
    Frontend -.-> History
    Frontend -.-> Admin
    
    Backend -.-> Auth
    Backend -.-> Docs
    Backend -.-> Prompt
    Backend -.-> AdminAPI

    style Frontend fill:#E8F0FE,stroke:#5F8DFF,stroke-width:2px
    style Backend fill:#E9F7EF,stroke:#3FA796,stroke-width:2px
    style Database fill:#FFF4E6,stroke:#FF9800,stroke-width:2px
    style Gemini fill:#F3E5F5,stroke:#9C27B0,stroke-width:2px
```

**ASCII Fallback:**

```text
┌─────────────────────────────────────────────────────────────────┐
│                    Presentation Layer                            │
│              React.js with TypeScript Frontend                  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐        │
│  │  Login   │  │  Email   │  │  Report  │  │  History │        │
│  │ Register │  │ Generator│  │ Generator│  │  Admin   │        │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘        │
└───────────────────────┬─────────────────────────────────────────┘
                        │ REST API (HTTP/JSON)
                        │ JWT Authentication
                        ↓
┌─────────────────────────────────────────────────────────────────┐
│                    Application Layer                             │
│                      Flask REST API                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ Auth Service │  │  Document    │  │   Prompt     │          │
│  │ JWT + RBAC   │  │  Generation  │  │   Engine    │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└───────────┬──────────────────────────────┬──────────────────────┘
            │                              │
            │ ORM/SQL                      │ API Calls
            ↓                              ↓
┌───────────────────────┐    ┌──────────────────────────────────┐
│     Data Layer        │    │      AI Service Layer            │
│   PostgreSQL          │    │   Google Gemini API              │
│                       │    │                                  │
│  • Users              │    │  • Content Generation            │
│  • Documents          │    │  • Prompt Processing             │
│  • Audit Logs         │    │  • Response Handling             │
└───────────────────────┘    └──────────────────────────────────┘
```

### Architecture Layers

- **Presentation Layer**: React.js with TypeScript SPA for user interaction
- **Application Layer**: Flask REST API handling business logic and security
- **Data Layer**: PostgreSQL for persistent storage
- **AI Service Layer**: Google Gemini for text generation

---

## 🧰 Technology Stack

| Layer | Technology |
|-----|-----------|
| Frontend | React.js with TypeScript |
| Backend | Python (Flask) |
| Database | PostgreSQL |
| Authentication | JWT |
| AI Model | Google Gemini |
| Version Control | GitHub |

---

## 🔐 Authentication & Authorization

For complete authentication and authorization documentation, see [Authentication & Authorization Guide](docs/08_authentication_authorization.md).

**Quick Overview:**

- **JWT-based authentication** - Stateless, secure token-based authentication
- **Secure password hashing** - Industry-standard password security
- **Role-Based Access Control (RBAC)** - User and Admin roles with differentiated permissions
- **Audit logging** - Comprehensive authentication event tracking

---

## 🧠 Prompt Engineering Strategy

For complete prompt engineering documentation, see [Prompt Engineering Strategy Guide](docs/09_prompt_engineering_strategy.md).

**Quick Overview:**

- **Instruction-Based Prompting** - Optimized for Google Gemini's instruction-following capabilities
- **Structured Prompts** - Role definition, task description, tone constraints, and format guidance
- **Tone Control** - Professional, formal, casual, and friendly tone options
- **Report Structures** - Executive summary, detailed, and bullet point formats

---

## 🗄️ Database Schema (Simplified)

### Users

- id
- username
- email
- password_hash
- role
- created_at

### Generated Documents

- id
- user_id
- doc_type
- title
- prompt_input
- content
- tone
- structure
- created_at

---

## 📊 Evaluation Metrics

- Content relevance and coherence
- Tone accuracy
- Time efficiency compared to manual drafting
- User satisfaction
- System reliability

---

## ⚠️ Limitations

- Output quality depends on input clarity
- No external fact verification
- Dependent on Gemini API availability

---

## 🚀 Future Enhancements

- Integration with enterprise IAM (Auth0 / Azure AD)
- Analytics dashboard
- PDF / Email export
- Retrieval-Augmented Generation (RAG)
- Multilingual content generation
- Usage-based recommendations

---

## 📂 Repository Structure

For a complete and detailed repository structure, see [Repository Structure Documentation](docs/07_repository_structure.md).

**Quick Overview:**

- `frontend/` - React.js with TypeScript (Presentation Layer)
- `backend/` - Flask REST API (Application Layer)
- `database/` - PostgreSQL schema (Data Layer)
- `docs/` - Project documentation
- `tools/` - Utility scripts and automation

---

---

## 🚀 Quick Start

### Prerequisites

- Python 3.9+
- Node.js 18+
- PostgreSQL 13+

### Backend Setup

#### Option 1: Using `uv` (Recommended - Faster)

**Directory:** `backend/`  
**Environment:** Windows 11 PowerShell

```powershell
# Install uv first (one-time setup)
irm https://astral.sh/uv/install.ps1 | iex

# Navigate to backend directory
cd backend

# Create virtual environment and install dependencies
uv venv
uv pip install -r requirements.txt

# Activate virtual environment
.venv\Scripts\Activate.ps1

# Set environment variables
$env:DATABASE_URL = "postgresql://user:password@localhost/genai_email_report"
$env:JWT_SECRET_KEY = "your-secret-key"
$env:GEMINI_API_KEY = "your-gemini-api-key"

# Run the Flask server
python app.py
```

#### Option 2: Using Traditional pip/venv

**Directory:** `backend/`  
**Environment:** Windows 11 PowerShell

```powershell
# Navigate to backend directory
cd backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
.\venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt

# Set environment variables
$env:DATABASE_URL = "postgresql://user:password@localhost/genai_email_report"
$env:JWT_SECRET_KEY = "your-secret-key"
$env:GEMINI_API_KEY = "your-gemini-api-key"

# Run the Flask server
python app.py
```

### Frontend Setup

**Directory:** `frontend/`  
**Environment:** Windows 11 PowerShell

```powershell
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Create .env file with:
# VITE_API_BASE_URL=http://localhost:5000/api

# Run the development server
npm run dev
```

The application will be available at:

- Frontend: `http://localhost:5173`
- Backend API: `http://localhost:5000`

### Build for Production

**Directory:** `frontend/`  
**Environment:** Windows 11 PowerShell

```powershell
# Navigate to frontend directory
cd frontend

# Build for production
npm run build
```

---

## 📖 Documentation

- Full project documentation available in `/docs`
- Frontend documentation in `/frontend/README.md`
- Backend documentation in `/backend/README.md`
- Database schema in `/database/README.md`
- IEEE / college-format report included
- Architecture diagrams and module descriptions provided

---

## 📜 License

This project is intended for **academic and educational use**.

---

## 🙌 Acknowledgements

- Google Gemini API
- Flask & React open-source communities
- PostgreSQL Documentation

---

## 📧 Contact

For queries or collaboration, please reach out via GitHub issues or pull requests.

---

**⭐ If you find this project useful, feel free to star the repository.**

---
