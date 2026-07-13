# Hospital Ultrasound Report Generator

A comprehensive web application for private hospitals to manage ultrasound imaging, generate automated reports, and provide secure access to patients and medical professionals.

## Features

- 👥 **Patient Management System** - Register, manage, and track patient information
- 📷 **Image Upload & Storage** - Secure storage for ultrasound images
- 📄 **Automated Report Generation** - Generate professional ultrasound reports
- 👨‍⚕️ **Doctor Dashboard** - Manage patients, upload images, create reports
- 🔐 **Secure Authentication** - Role-based access control (Admin, Doctor, Patient)
- 📱 **Patient Portal** - View reports, download PDF, medical history
- 📧 **Email Notifications** - Automated email alerts for patients
- 📊 **Admin Panel** - Manage users, hospitals, and system settings
- 🔄 **DICOM Support** - Support for medical imaging standards
- 📥 **PDF Export** - Generate and download professional reports

## Tech Stack

### Frontend
- **React.js** - Modern UI framework
- **Tailwind CSS** - Responsive styling
- **Axios** - HTTP client
- **React Router** - Navigation
- **Redux** - State management

### Backend
- **Node.js & Express.js** - REST API server
- **PostgreSQL** - Primary database
- **JWT** - Authentication
- **Multer** - File upload handling
- **Nodemailer** - Email service
- **Sharp** - Image processing

### DevOps
- **Docker & Docker Compose** - Containerization
- **Nginx** - Reverse proxy
- **Environment Variables** - Configuration management

## Project Structure

```
hospital-ultrasound-website/
├── frontend/                 # React.js application
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── redux/
│   │   ├── App.jsx
│   │   └── index.js
│   ├── package.json
│   └── Dockerfile
├── backend/                  # Node.js/Express API
│   ├── config/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── middleware/
│   ├── services/
│   ├── uploads/
│   ├── .env.example
│   ├── server.js
│   ├── package.json
│   └── Dockerfile
├── database/                 # Database schemas & migrations
│   ├── migrations/
│   ├── seeds/
│   └── schema.sql
├── docs/                     # Documentation
│   ├── API.md
│   ├── SETUP.md
│   ├── DATABASE.md
│   └── DEPLOYMENT.md
├── .gitignore
├── docker-compose.yml
├── .env.example
└── README.md
```

## Quick Start

### Prerequisites
- Node.js (v16+)
- PostgreSQL (v12+)
- Docker & Docker Compose (optional)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/mairidarsgahschool-commits/hospital-ultrasound-website.git
   cd hospital-ultrasound-website
   ```

2. **Setup with Docker (Recommended)**
   ```bash
   docker-compose up -d
   ```

3. **Manual Setup**

   **Backend Setup:**
   ```bash
   cd backend
   npm install
   cp .env.example .env
   # Configure database and email settings in .env
   npm run migrate
   npm start
   ```

   **Frontend Setup:**
   ```bash
   cd frontend
   npm install
   npm start
   # Application runs on http://localhost:3000
   ```

4. **Access the Application**
   - Frontend: http://localhost:3000
   - API: http://localhost:5000
   - Admin Panel: http://localhost:3000/admin

## User Roles

### 1. **Admin**
   - Manage hospital staff
   - Manage departments
   - View all reports
   - System configuration

### 2. **Doctor/Radiologist**
   - Upload ultrasound images
   - Create and edit reports
   - View patient history
   - Generate PDF reports

### 3. **Patient**
   - View personal reports
   - Download reports (PDF)
   - Medical history
   - Appointment scheduling

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/me` - Get current user
- `POST /api/auth/refresh` - Refresh JWT token

### Patients
- `GET /api/patients` - List all patients
- `POST /api/patients` - Create new patient
- `GET /api/patients/:id` - Get patient details
- `PUT /api/patients/:id` - Update patient
- `DELETE /api/patients/:id` - Delete patient

### Ultrasound Images
- `POST /api/images/upload` - Upload ultrasound image
- `GET /api/images/:id` - Get image details
- `DELETE /api/images/:id` - Delete image

### Reports
- `POST /api/reports` - Create new report
- `GET /api/reports/:id` - Get report details
- `PUT /api/reports/:id` - Update report
- `GET /api/reports/:id/pdf` - Download PDF
- `DELETE /api/reports/:id` - Delete report

## Database Schema

Key tables:
- `users` - Hospital staff and patients
- `patients` - Patient information
- `ultrasound_images` - Stored images
- `reports` - Generated reports
- `departments` - Hospital departments
- `user_roles` - User permissions

## Security Features

✅ JWT token-based authentication
✅ Password hashing with bcrypt
✅ Role-based access control (RBAC)
✅ Data encryption at rest
✅ Secure file upload validation
✅ HIPAA-compliant patient data handling
✅ SQL injection prevention
✅ CORS protection
✅ Rate limiting

## Environment Variables

Create a `.env` file in the backend directory:

```env
# Server
NODE_ENV=development
PORT=5000
SECRET_KEY=your_secret_key_here

# Database
DB_HOST=postgres
DB_PORT=5432
DB_NAME=hospital_ultrasound
DB_USER=postgres
DB_PASSWORD=postgres_password

# JWT
JWT_SECRET=your_jwt_secret
JWT_EXPIRY=7d

# Email
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password

# File Upload
MAX_FILE_SIZE=50mb
UPLOAD_DIR=./uploads

# Frontend URL
FRONTEND_URL=http://localhost:3000
```

## Running with Docker

```bash
# Build and start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Run migrations
docker-compose exec backend npm run migrate
```

## Development

```bash
# Backend development
cd backend
npm run dev

# Frontend development
cd frontend
npm start
```

## Testing

```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd frontend
npm test
```

## Deployment

See [DEPLOYMENT.md](docs/DEPLOYMENT.md) for detailed deployment instructions for:
- AWS EC2
- Heroku
- DigitalOcean
- Docker Hub

## Contributing

1. Create a feature branch
2. Commit your changes
3. Push to the branch
4. Create a Pull Request

## License

MIT License - See LICENSE file for details

## Support

For support, email: support@hospital-ultrasound.com

## Roadmap

- [ ] AI-powered report suggestions
- [ ] Video consultation integration
- [ ] Mobile app (iOS/Android)
- [ ] Advanced analytics dashboard
- [ ] Integration with hospital EHR systems
- [ ] Multi-language support
- [ ] Blockchain-based report verification

---

**Last Updated:** 2026-07-13
