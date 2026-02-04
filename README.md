# FastAPI Auth API

![Python](https://img.shields.io/badge/python-3.11+-blue.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-0.109.0-green.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

Production-grade REST API with JWT authentication, rate limiting, and comprehensive security features.

## Features

- ✅ **JWT Authentication** - Secure token-based auth
- ✅ **Rate Limiting** - Prevent API abuse (60 req/min)
- ✅ **Password Hashing** - Bcrypt encryption
- ✅ **PostgreSQL** - Production database
- ✅ **SQLAlchemy ORM** - Database management
- ✅ **Pydantic Validation** - Request/response validation
- ✅ **API Documentation** - Auto-generated Swagger/ReDoc
- ✅ **Comprehensive Tests** - pytest with 85%+ coverage
- ✅ **Docker Support** - Easy deployment

## Quick Start

### Using Docker (Recommended)
```bash
# Start services
docker-compose up -d

# Install dependencies
pip install -r requirements.txt

# Run API
uvicorn app.main:app --reload

# API runs at: http://localhost:8000
# Docs at: http://localhost:8000/docs
```

### Manual Setup
```bash
# Install dependencies
pip install -r requirements.txt

# Set up environment
cp .env.example .env
# Edit .env with your configuration

# Start PostgreSQL and Redis
# (or use docker-compose for just databases)

# Run migrations (optional - auto-created on startup)
# alembic upgrade head

# Start API
uvicorn app.main:app --reload --port 8000
```

## API Endpoints

### Authentication
```bash
# Register new user
POST /api/v1/auth/register
{
  "email": "user@example.com",
  "username": "johndoe",
  "password": "securepass123"
}

# Login
POST /api/v1/auth/login
Form data:
  username: johndoe
  password: securepass123

Response:
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer"
}
```

### Users (Authenticated)
```bash
# Get current user
GET /api/v1/users/me
Headers: Authorization: Bearer <token>

# Update current user
PUT /api/v1/users/me
Headers: Authorization: Bearer <token>
{
  "email": "newemail@example.com"
}

# Delete account
DELETE /api/v1/users/me
Headers: Authorization: Bearer <token>
```

## Testing
```bash
# Run all tests
pytest tests/ -v

# With coverage
pytest tests/ --cov=app --cov-report=html

# Run specific test file
pytest tests/test_auth.py -v
```

## Project Structure
```
fastapi-auth-api/
├── app/
│   ├── api/
│   │   ├── v1/
│   │   │   ├── endpoints/
│   │   │   │   ├── auth.py      # Auth endpoints
│   │   │   │   └── users.py     # User endpoints
│   │   │   └── router.py        # API router
│   │   └── dependencies.py      # Auth dependencies
│   ├── core/
│   │   ├── config.py           # Settings
│   │   └── security.py         # JWT & password utils
│   ├── db/
│   │   └── database.py         # Database connection
│   ├── models/
│   │   └── user.py             # SQLAlchemy models
│   ├── schemas/
│   │   └── user.py             # Pydantic schemas
│   ├── services/
│   │   └── user_service.py     # Business logic
│   └── main.py                 # FastAPI app
├── tests/
│   ├── test_auth.py
│   └── test_users.py
├── docker-compose.yml
├── requirements.txt
└── README.md
```

## Security Features

### JWT Authentication
- Token-based authentication
- Configurable expiration (default: 30 min)
- HS256 algorithm
- Secure secret key

### Password Security
- Bcrypt hashing
- Password strength validation
- No plain-text storage

### Rate Limiting
- 60 requests/minute per IP
- Prevents brute force attacks
- Customizable limits

### Database Security
- Parameterized queries (SQL injection prevention)
- Connection pooling
- Environment-based credentials

## Configuration

Environment variables (`.env`):
```env
DATABASE_URL=postgresql://user:password@localhost:5432/authdb
SECRET_KEY=your-super-secret-key-change-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
REDIS_URL=redis://localhost:6379/0
RATE_LIMIT_PER_MINUTE=60
```

## Deployment

### Docker
```bash
# Build and run
docker-compose up --build

# Run in background
docker-compose up -d

# View logs
docker-compose logs -f
```

### Production Checklist

- [ ] Change `SECRET_KEY` to strong random value
- [ ] Use production database (not SQLite)
- [ ] Enable HTTPS
- [ ] Set up proper CORS
- [ ] Configure logging
- [ ] Set up monitoring (Sentry, Datadog)
- [ ] Use environment variables for secrets
- [ ] Set up CI/CD pipeline

## Interview Talking Points

### Architecture
- **Clean Architecture**: Separation of concerns (routes, services, models)
- **Dependency Injection**: FastAPI's dependency system
- **Repository Pattern**: UserService encapsulates DB logic

### Security
- **JWT vs Sessions**: Stateless authentication, scalable
- **Password Hashing**: One-way encryption with bcrypt
- **Rate Limiting**: Prevent abuse and DDoS

### Database
- **ORM vs Raw SQL**: SQLAlchemy for type safety and migrations
- **Connection Pooling**: Efficient DB connections
- **Migrations**: Alembic for schema changes

### Testing
- **Test Fixtures**: Pytest for clean test setup
- **Test Database**: Isolated SQLite for tests
- **Coverage**: 85%+ code coverage

## Performance

- **Async/Await**: Non-blocking I/O
- **Connection Pooling**: Reuse DB connections
- **Caching**: Redis for rate limiting
- **Indexing**: Database indexes on email/username

## License

MIT License

## Author

[Tu Nombre] - Backend Engineer

---

**Built with:** FastAPI, PostgreSQL, SQLAlchemy, JWT, Redis
