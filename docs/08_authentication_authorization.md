# Authentication & Authorization

**Project:** GenAI Email & Report Drafting System  
**Last Updated:** 2026-01-09  
**Status:** Implemented

---

## Overview

The GenAI Email & Report Drafting System implements a secure, stateless authentication mechanism using **JSON Web Tokens (JWT)** with **Role-Based Access Control (RBAC)**. This approach provides enterprise-grade security while maintaining simplicity and scalability.

---

## Authentication Architecture

### JWT-Based Authentication

The system uses **stateless JWT-based authentication** for secure API access:

- **Token-Based:** Users receive a JWT token upon successful login
- **Stateless:** No server-side session storage required
- **Secure:** Tokens are signed with a secret key and include expiration
- **Scalable:** Enables horizontal scaling without session synchronization

### Why JWT Instead of Third-Party IAM?

Third-party IAM solutions such as Auth0 were intentionally excluded to:

- **Reduce Complexity:** Eliminate external dependencies and configuration overhead
- **Ensure Demo Reliability:** Avoid external service dependencies for demonstrations
- **Maintain Control:** Full control over authentication logic and user management
- **Academic Suitability:** JWT provides sufficient security for the current scope

JWT authentication provides a **stateless, lightweight, and fully controlled** security mechanism that is well-suited for academic and prototype-level enterprise systems.

---

## Authentication Flow

### 1. User Registration

**Endpoint:** `POST /api/auth/register`

**Request:**

```json
{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "SecurePass123",
  "role": "USER"
}
```

**Response:**

```json
{
  "message": "User registered successfully",
  "user": {
    "id": 1,
    "username": "johndoe",
    "email": "john@example.com",
    "role": "USER"
  }
}
```

**Password Requirements:**

- Minimum 8 characters
- At least one letter
- At least one digit

### 2. User Login

**Endpoint:** `POST /api/auth/login`

**Request:**

```json
{
  "username": "johndoe",
  "password": "SecurePass123"
}
```

**Response:**

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "username": "johndoe",
    "email": "john@example.com",
    "role": "USER"
  }
}
```

**Token Details:**

- **Expiration:** 24 hours (configurable)
- **Claims:** User ID and role
- **Storage:** Frontend stores token in localStorage

### 3. Protected Endpoint Access

**Header Format:**

```text
Authorization: Bearer {jwt_token}
```

**Example:**

```http
GET /api/documents/history HTTP/1.1
Host: localhost:5000
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## Password Security

### Secure Password Hashing

- **Algorithm:** Werkzeug's `pbkdf2:sha256` (industry-standard)
- **Salt:** Automatically generated per password
- **Storage:** Only hashed passwords stored in database
- **Validation:** `check_password_hash()` for secure comparison

**Implementation:**

```python
from werkzeug.security import generate_password_hash, check_password_hash

# Hash password during registration
password_hash = generate_password_hash(password)

# Verify password during login
is_valid = check_password_hash(user.password_hash, provided_password)
```

---

## Role-Based Access Control (RBAC)

The system implements **two-tier role-based access control**:

### User Roles

#### 1. **USER** (Default Role)

**Permissions:**

- ✅ Login / Register
- ✅ Generate emails and reports
- ✅ View personal generation history
- ✅ Access own profile information

**Restrictions:**

- ❌ Cannot access admin endpoints
- ❌ Cannot view other users' documents
- ❌ Cannot access system statistics

#### 2. **ADMIN** (Elevated Role)

**Permissions:**

- ✅ All User permissions
- ✅ View all generated documents (system-wide)
- ✅ Access system usage data (read-only)
- ✅ View audit logs
- ✅ Access admin dashboard

**Admin-Only Endpoints:**

- `GET /api/admin/stats` - System statistics
- `GET /api/admin/users` - User management
- `GET /api/admin/documents` - All documents
- `GET /api/admin/audit-logs` - Audit log access

---

## Implementation Details

### Backend Implementation

#### JWT Configuration (`backend/app.py`)

```python
from flask_jwt_extended import JWTManager

# Initialize JWT
jwt = JWTManager(app)

# JWT Configuration (in config.py)
JWT_SECRET_KEY = os.getenv("JWT_SECRET_KEY")
JWT_ACCESS_TOKEN_EXPIRES = timedelta(hours=24)
```

#### Authentication Routes (`backend/routes/auth.py`)

- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User authentication
- `GET /api/auth/me` - Get current user (protected)

#### Role Enforcement (`backend/routes/admin.py`)

```python
def admin_required():
    """Decorator to require admin role."""
    def decorator(fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            jwt_data = get_jwt()
            user_role = jwt_data.get('role', 'USER')
            
            if user_role != 'ADMIN':
                return jsonify({'error': 'Admin access required'}), 403
                
            return fn(*args, **kwargs)
        return wrapper
    return decorator

# Usage
@admin_bp.route('/stats', methods=['GET'])
@jwt_required()
@admin_required()
def get_stats():
    # Admin-only endpoint
    pass
```

### Frontend Implementation

#### Token Storage (`frontend/src/api/client.ts`)

```typescript
// Store token after login
localStorage.setItem('token', access_token);

// Include token in API requests
headers: {
  'Authorization': `Bearer ${token}`
}
```

#### Route Protection (`frontend/src/components/PrivateRoute.tsx`)

- Checks for valid JWT token
- Redirects to login if unauthenticated
- Protects routes requiring authentication

#### Admin Route Protection (`frontend/src/components/AdminRoute.tsx`)

- Validates user role from JWT
- Restricts access to ADMIN role only
- Provides role-based UI customization

---

## Security Features

### 1. Token Expiration

- **Default:** 24 hours
- **Automatic:** Tokens expire and require re-authentication
- **Error Handling:** Clear error messages for expired tokens

### 2. Password Validation

- **Strength Requirements:** Enforced during registration
- **Email Validation:** Format validation for email addresses
- **Input Sanitization:** Prevents injection attacks

### 3. Audit Logging

All authentication events are logged:

- ✅ Successful logins
- ✅ Failed login attempts
- ✅ User registrations
- ✅ Token validations
- ✅ Role-based access attempts

**Audit Log Model:**

```python
class AuditLog(db.Model):
    user_id: int
    action: str  # 'login_success', 'login_failed', etc.
    details: str
    timestamp: datetime
```

### 4. Error Handling

**Authentication Errors:**

- `401 Unauthorized` - Invalid credentials or expired token
- `403 Forbidden` - Insufficient permissions (role-based)
- `400 Bad Request` - Invalid input or missing fields

**Error Response Format:**

```json
{
  "error": "Invalid username or password",
  "message": "Authentication failed"
}
```

---

## Configuration

### Environment Variables

**Required:**

```powershell
# Backend .env file
JWT_SECRET_KEY=your-secret-key-here  # Must be strong and secret
DATABASE_URL=postgresql://user:password@localhost/genai_email_report
```

**Security Best Practices:**

- Use strong, randomly generated `JWT_SECRET_KEY`
- Never commit secrets to version control
- Use environment variables or secret management services
- Rotate secrets periodically in production

---

## API Endpoints Summary

### Public Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/auth/register` | Register new user |
| `POST` | `/api/auth/login` | Authenticate user |

### Protected Endpoints (Require JWT)

| Method | Endpoint | Role | Description |
|--------|----------|------|-------------|
| `GET` | `/api/auth/me` | USER/ADMIN | Get current user |
| `POST` | `/api/documents/generate` | USER/ADMIN | Generate document |
| `GET` | `/api/documents/history` | USER/ADMIN | Get user history |
| `GET` | `/api/admin/stats` | ADMIN | System statistics |
| `GET` | `/api/admin/users` | ADMIN | List all users |
| `GET` | `/api/admin/documents` | ADMIN | All documents |

---

## Testing

### Authentication Tests (`backend/tests/test_auth.py`)

- ✅ User registration validation
- ✅ Password hashing verification
- ✅ JWT token generation
- ✅ Token expiration handling
- ✅ Role-based access control
- ✅ Admin endpoint protection
- ✅ Failed login attempt logging

**Run Tests:**

```powershell
cd backend
pytest tests/test_auth.py -v
```

---

## Related Documentation

- [Repository Structure](07_repository_structure.md) - Project structure
- [Requirements Specification](02_requirements.md) - System requirements
- [Technical Documentation](04_technical.md) - Technical implementation
- [Architecture Plan](05_architecture_plan.md) - System architecture

---

**Last Updated:** 2026-01-09  
**Maintained By:** Viswanatha Swamy P K
