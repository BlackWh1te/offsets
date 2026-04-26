# VirtualOffice Setup Guide

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- Python 3.11+
- Redis (optional, for real-time features)

### 1. Backend Setup

```bash
cd ai-service

# Create virtual environment
python -m venv venv
venv\Scripts\activate  # On Windows

# Install dependencies
pip install -r requirements.txt

# Configure environment
copy .env.example .env
# Edit .env and set JWT_SECRET_KEY (generate with: python -c "import secrets; print(secrets.token_urlsafe(32))")

# Start server
uvicorn src.main:app --reload --host 0.0.0.0 --port 8000
```

### 2. Frontend Setup

```bash
cd web

# Install dependencies
npm install

# Configure environment
copy .env.example .env.local
# Edit .env.local if needed

# Initialize database
npx prisma generate
npx prisma db push

# Start development server
npm run dev
```

### 3. Access Application
- Frontend: http://localhost:3000
- Backend API: http://localhost:8000
- API Docs: http://localhost:8000/docs

## 🔒 Security Configuration (CRITICAL)

### Required Environment Variables

**Backend (`ai-service/.env`):**
```env
# REQUIRED - Generate with: python -c "import secrets; print(secrets.token_urlsafe(32))"
JWT_SECRET_KEY=<your-strong-random-key-here>

# LLM Configuration (at least one required)
OPENAI_API_KEY=sk-your-key-here
OPENAI_MODEL=gpt-4o-mini

# Redis Configuration (optional for development)
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_DB=0
REDIS_PASSWORD=

# CORS Configuration
ALLOWED_ORIGINS=http://localhost:3000
```

**Frontend (`web/.env.local`):**
```env
DATABASE_URL="file:./dev.db"
NEXT_PUBLIC_API_URL=http://localhost:8000
```

## ✅ Security Fixes Applied

This setup includes the following security improvements:

1. ✅ **Prisma Database Configuration** - Proper DATABASE_URL configuration
2. ✅ **No Hardcoded Credentials** - Authentication requires database setup
3. ✅ **Strong JWT Secret** - Enforced via environment variable validation
4. ✅ **Token Security** - Tokens passed in headers, not URLs
5. ✅ **Input Validation** - Zod validation on all API inputs
6. ✅ **CORS Configuration** - Environment-based origin control
7. ✅ **Redis Error Handling** - Graceful degradation when Redis unavailable
8. ✅ **Modern DateTime** - Using timezone-aware datetime functions

## 🐛 Troubleshooting

### Backend won't start
- Check JWT_SECRET_KEY is set in `.env`
- Verify Python dependencies are installed
- Check port 8000 is not in use

### Frontend won't start
- Run `npm install` to ensure all dependencies are installed
- Check DATABASE_URL is set in `.env.local`
- Verify backend is running on port 8000

### Memory issues
- See `MEMORY-LEAK-FIX.md` for frontend memory troubleshooting
- Kill runaway processes: `taskkill /F /IM node.exe`

## 📚 Additional Documentation

- `MEMORY-LEAK-FIX.md` - Frontend memory issue resolution
- `PHASE2-QUICKSTART.md` - Phase 2 features setup
- `QUICK-FIX.md` - Common issues and solutions
- `TEST-PHASE2.md` - Testing Phase 2 features

## 🔐 Production Deployment Checklist

- [ ] Generate strong JWT_SECRET_KEY (32+ characters)
- [ ] Set up HTTPS/TLS
- [ ] Configure proper CORS origins (no wildcards)
- [ ] Enable rate limiting
- [ ] Set up Redis for production
- [ ] Configure database backups
- [ ] Set up monitoring and logging
- [ ] Regular security updates
- [ ] Remove demo/test credentials
- [ ] Enable audit logging

## 📝 Notes

- Authentication is currently disabled pending database user setup
- Redis is optional for development but recommended for production
- All Phase 2 endpoints require authentication
- See individual bug fix comments in code for implementation details
