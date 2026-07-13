# Setup Guide - Hospital Ultrasound Report Generator

## Prerequisites

- Node.js v16.0.0 or higher
- PostgreSQL 12 or higher
- npm or yarn package manager
- Git
- Docker & Docker Compose (optional)

## Installation Steps

### Option 1: Docker Setup (Recommended)

This is the quickest way to get the application running.

#### Step 1: Clone the Repository

```bash
git clone https://github.com/mairidarsgahschool-commits/hospital-ultrasound-website.git
cd hospital-ultrasound-website
```

#### Step 2: Configure Environment

```bash
cp .env.example .env
```

Edit `.env` and configure:
- Database credentials
- JWT secret
- Email configuration
- File upload paths

#### Step 3: Start with Docker Compose

```bash
docker-compose up -d
```

Wait for all services to be healthy:

```bash
docker-compose ps
```

All services should show "Up" status.

#### Step 4: Initialize Database

```bash
docker-compose exec backend npm run migrate
docker-compose exec backend npm run seed
```

#### Step 5: Access the Application

- Frontend: http://localhost:3000
- Backend API: http://localhost:5000
- API Docs: http://localhost:5000/api/docs

---

### Option 2: Manual Setup

For development or custom configurations.

#### Step 1: Clone the Repository

```bash
git clone https://github.com/mairidarsgahschool-commits/hospital-ultrasound-website.git
cd hospital-ultrasound-website
```

#### Step 2: Setup PostgreSQL

```bash
# Create database
createdb hospital_ultrasound

# Run schema migrations
psql hospital_ultrasound < database/schema.sql
```

#### Step 3: Backend Setup

```bash
cd backend

# Install dependencies
npm install

# Configure environment
cp .env.example .env

# Edit .env with your settings
nano .env

# Run migrations
npm run migrate

# Seed initial data (optional)
npm run seed

# Start the server
npm start

# For development with auto-reload
npm run dev
```

Backend will run on http://localhost:5000

#### Step 4: Frontend Setup

In a new terminal:

```bash
cd frontend

# Install dependencies
npm install

# Configure environment
cp .env.example .env

# Edit .env if needed
nano .env

# Start development server
npm start
```

Frontend will run on http://localhost:3000

---

## Configuration

### Backend Configuration (.env)

```env
# Server
NODE_ENV=development
PORT=5000

# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=hospital_ultrasound
DB_USER=postgres
DB_PASSWORD=your_password

# JWT
JWT_SECRET=your_super_secret_jwt_key
JWT_EXPIRY=7d

# Email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password

# Frontend URL
FRONTEND_URL=http://localhost:3000
```

### Frontend Configuration (.env)

```env
REACT_APP_API_URL=http://localhost:5000/api
REACT_APP_NAME=Hospital Ultrasound Report Generator
```

## Initial Admin User

After setup, create your admin user:

```bash
cd backend
node scripts/create-admin.js
```

Follow the prompts to set:
- Email
- Password
- Full name

## Default Credentials (if using seed)

After running `npm run seed`:

**Admin User:**
- Email: admin@hospital.com
- Password: Admin@123

**Doctor User:**
- Email: doctor@hospital.com
- Password: Doctor@123

**Patient User:**
- Email: patient@hospital.com
- Password: Patient@123

⚠️ **IMPORTANT:** Change these credentials immediately in production!

## Verify Installation

### Check Backend

```bash
curl http://localhost:5000/api/health
```

Should return:
```json
{"status": "ok", "timestamp": "..."}
```

### Check Frontend

Open http://localhost:3000 in your browser. You should see the login page.

### Check Database Connection

```bash
psql -h localhost -U postgres -d hospital_ultrasound -c "SELECT version();"
```

## Troubleshooting

### Port Already in Use

If ports 3000 or 5000 are already in use:

```bash
# Find process using port
lsof -i :3000
lsof -i :5000

# Kill process
kill -9 <PID>
```

Or change PORT in `.env` and restart.

### Database Connection Error

```bash
# Test connection
psql -h localhost -U postgres -d hospital_ultrasound

# Check credentials in .env
# Ensure PostgreSQL is running
```

### Module Not Found Error

```bash
# Clear node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

### Docker Issues

```bash
# Check logs
docker-compose logs backend
docker-compose logs frontend

# Rebuild containers
docker-compose down
docker-compose up -d --build
```

## Next Steps

1. Review [API Documentation](API.md)
2. Customize database schema in `database/schema.sql`
3. Configure email service
4. Set up S3 for image storage (optional)
5. Configure CORS settings
6. Set up SSL certificates

## Development Commands

### Backend

```bash
cd backend

# Run in development mode
npm run dev

# Run tests
npm test

# Run linter
npm run lint

# Check code style
npm run format
```

### Frontend

```bash
cd frontend

# Run development server
npm start

# Run tests
npm test

# Build for production
npm run build

# Run linter
npm run lint
```

## Additional Resources

- [Database Schema Documentation](DATABASE.md)
- [API Reference](API.md)
- [Deployment Guide](DEPLOYMENT.md)
- [Environment Variables](../.env.example)

## Support

If you encounter issues:

1. Check the [troubleshooting section](#troubleshooting)
2. Review application logs
3. Check GitHub issues
4. Contact support team

---

**Last Updated:** 2026-07-13
