# API Documentation - Hospital Ultrasound Report Generator

## Base URL

```
http://localhost:5000/api
```

## Authentication

All endpoints (except auth endpoints) require a Bearer token in the Authorization header:

```
Authorization: Bearer <jwt_token>
```

---

## Authentication Endpoints

### Register User

```http
POST /auth/register
Content-Type: application/json
```

**Request Body:**

```json
{
  "email": "user@hospital.com",
  "password": "SecurePassword123!",
  "first_name": "John",
  "last_name": "Doe",
  "role": "patient",
  "phone": "+1-234-567-8900"
}
```

**Response (201):**

```json
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@hospital.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "patient",
    "created_at": "2026-07-13T10:30:00Z"
  }
}
```

### Login

```http
POST /auth/login
Content-Type: application/json
```

**Request Body:**

```json
{
  "email": "user@hospital.com",
  "password": "SecurePassword123!"
}
```

**Response (200):**

```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "user": {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "email": "user@hospital.com",
      "first_name": "John",
      "role": "patient"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": "7d"
  }
}
```

### Get Current User

```http
GET /auth/me
Authorization: Bearer <token>
```

**Response (200):**

```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "email": "user@hospital.com",
    "first_name": "John",
    "role": "patient",
    "is_active": true
  }
}
```

### Logout

```http
POST /auth/logout
Authorization: Bearer <token>
```

**Response (200):**

```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

---

## Patient Endpoints

### List All Patients

```http
GET /patients?page=1&limit=20
Authorization: Bearer <token>
```

**Query Parameters:**
- `page` (integer) - Page number, default: 1
- `limit` (integer) - Items per page, default: 20, max: 100
- `search` (string) - Search by name or MRN

**Response (200):**

```json
{
  "success": true,
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440001",
      "user_id": "550e8400-e29b-41d4-a716-446655440000",
      "medical_record_number": "MRN-001",
      "first_name": "Jane",
      "last_name": "Smith",
      "date_of_birth": "1990-05-15",
      "gender": "female",
      "blood_type": "O+",
      "city": "New York",
      "state": "NY"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "pages": 8
  }
}
```

### Get Patient Details

```http
GET /patients/{patient_id}
Authorization: Bearer <token>
```

**Response (200):**

```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "user_id": "550e8400-e29b-41d4-a716-446655440000",
    "medical_record_number": "MRN-001",
    "first_name": "Jane",
    "last_name": "Smith",
    "date_of_birth": "1990-05-15",
    "gender": "female",
    "blood_type": "O+",
    "address": "123 Main St",
    "city": "New York",
    "state": "NY",
    "postal_code": "10001",
    "country": "USA",
    "emergency_contact_name": "John Smith",
    "emergency_contact_phone": "+1-234-567-8900",
    "allergies": "Penicillin",
    "medical_history": "...",
    "insurance_provider": "BlueCross",
    "insurance_policy_number": "BC-123456",
    "created_at": "2026-07-01T10:00:00Z"
  }
}
```

### Create Patient

```http
POST /patients
Content-Type: application/json
Authorization: Bearer <token>
```

**Request Body:**

```json
{
  "medical_record_number": "MRN-002",
  "first_name": "Jane",
  "last_name": "Smith",
  "date_of_birth": "1990-05-15",
  "gender": "female",
  "blood_type": "O+",
  "address": "123 Main St",
  "city": "New York",
  "state": "NY",
  "postal_code": "10001",
  "country": "USA",
  "emergency_contact_name": "John Smith",
  "emergency_contact_phone": "+1-234-567-8900",
  "allergies": "Penicillin",
  "insurance_provider": "BlueCross",
  "insurance_policy_number": "BC-123456"
}
```

**Response (201):**

```json
{
  "success": true,
  "message": "Patient created successfully",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "medical_record_number": "MRN-002",
    "first_name": "Jane",
    "last_name": "Smith",
    "created_at": "2026-07-13T10:00:00Z"
  }
}
```

### Update Patient

```http
PUT /patients/{patient_id}
Content-Type: application/json
Authorization: Bearer <token>
```

**Request Body:** (same fields as create, all optional)

**Response (200):**

```json
{
  "success": true,
  "message": "Patient updated successfully",
  "data": { ... }
}
```

### Delete Patient

```http
DELETE /patients/{patient_id}
Authorization: Bearer <token>
```

**Response (200):**

```json
{
  "success": true,
  "message": "Patient deleted successfully"
}
```

---

## Ultrasound Image Endpoints

### Upload Image

```http
POST /images/upload
Content-Type: multipart/form-data
Authorization: Bearer <token>
```

**Form Parameters:**
- `file` (file, required) - Image file (JPEG, PNG, DICOM)
- `patient_id` (string, required) - Patient UUID
- `image_type` (string, required) - One of: abdomen, cardiac, obstetric, vascular, thyroid, breast, other
- `description` (string, optional) - Image description

**Response (201):**

```json
{
  "success": true,
  "message": "Image uploaded successfully",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440010",
    "patient_id": "550e8400-e29b-41d4-a716-446655440001",
    "file_name": "ultrasound_2026-07-13.jpg",
    "file_path": "/uploads/images/...",
    "file_size": 2048000,
    "image_type": "abdomen",
    "upload_date": "2026-07-13T10:30:00Z"
  }
}
```

### Get Image

```http
GET /images/{image_id}
Authorization: Bearer <token>
```

**Response (200):**

```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440010",
    "patient_id": "550e8400-e29b-41d4-a716-446655440001",
    "file_name": "ultrasound_2026-07-13.jpg",
    "file_size": 2048000,
    "image_type": "abdomen",
    "description": "Abdominal ultrasound",
    "upload_date": "2026-07-13T10:30:00Z"
  }
}
```

### List Patient Images

```http
GET /patients/{patient_id}/images
Authorization: Bearer <token>
```

**Response (200):**

```json
{
  "success": true,
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440010",
      "file_name": "ultrasound_2026-07-13.jpg",
      "image_type": "abdomen",
      "upload_date": "2026-07-13T10:30:00Z"
    }
  ]
}
```

---

## Ultrasound Report Endpoints

### Create Report

```http
POST /reports
Content-Type: application/json
Authorization: Bearer <token>
```

**Request Body:**

```json
{
  "patient_id": "550e8400-e29b-41d4-a716-446655440001",
  "ultrasound_image_id": "550e8400-e29b-41d4-a716-446655440010",
  "exam_type": "Abdominal Ultrasound",
  "clinical_indication": "Right upper quadrant pain",
  "findings": "The liver measures 14.5 cm...",
  "impression": "Normal liver and biliary system",
  "recommendations": "Follow-up as clinically indicated",
  "is_normal": true,
  "follow_up_required": false
}
```

**Response (201):**

```json
{
  "success": true,
  "message": "Report created successfully",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440020",
    "patient_id": "550e8400-e29b-41d4-a716-446655440001",
    "exam_type": "Abdominal Ultrasound",
    "status": "draft",
    "report_date": "2026-07-13T10:30:00Z",
    "created_at": "2026-07-13T10:30:00Z"
  }
}
```

### Get Report

```http
GET /reports/{report_id}
Authorization: Bearer <token>
```

**Response (200):**

```json
{
  "success": true,
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440020",
    "patient_id": "550e8400-e29b-41d4-a716-446655440001",
    "patient_name": "Jane Smith",
    "exam_type": "Abdominal Ultrasound",
    "clinical_indication": "Right upper quadrant pain",
    "findings": "The liver measures 14.5 cm...",
    "impression": "Normal liver and biliary system",
    "recommendations": "Follow-up as clinically indicated",
    "status": "final",
    "is_normal": true,
    "report_date": "2026-07-13T10:30:00Z",
    "created_by": "Dr. John Smith",
    "reviewed_by": "Dr. Jane Doe",
    "reviewed_at": "2026-07-13T11:00:00Z"
  }
}
```

### Update Report

```http
PUT /reports/{report_id}
Content-Type: application/json
Authorization: Bearer <token>
```

**Request Body:** (same as create, all optional)

**Response (200):**

```json
{
  "success": true,
  "message": "Report updated successfully",
  "data": { ... }
}
```

### Submit for Review

```http
POST /reports/{report_id}/submit-for-review
Authorization: Bearer <token>
```

**Response (200):**

```json
{
  "success": true,
  "message": "Report submitted for review",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440020",
    "status": "pending_review"
  }
}
```

### Approve Report

```http
POST /reports/{report_id}/approve
Authorization: Bearer <token>
```

**Response (200):**

```json
{
  "success": true,
  "message": "Report approved",
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440020",
    "status": "final",
    "reviewed_at": "2026-07-13T11:00:00Z"
  }
}
```

### Download PDF

```http
GET /reports/{report_id}/pdf
Authorization: Bearer <token>
```

**Response:** PDF file (application/pdf)

### List Patient Reports

```http
GET /patients/{patient_id}/reports
Authorization: Bearer <token>
```

**Query Parameters:**
- `status` - Filter by status (draft, pending_review, final, etc.)
- `limit` - Results per page
- `page` - Page number

**Response (200):**

```json
{
  "success": true,
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440020",
      "exam_type": "Abdominal Ultrasound",
      "status": "final",
      "report_date": "2026-07-13T10:30:00Z"
    }
  ]
}
```

---

## Error Responses

All errors follow this format:

```json
{
  "success": false,
  "message": "Error description",
  "error": "ERROR_CODE",
  "status": 400
}
```

### Common Error Codes

- `INVALID_REQUEST` (400) - Invalid request parameters
- `UNAUTHORIZED` (401) - Missing or invalid authentication
- `FORBIDDEN` (403) - User doesn't have permission
- `NOT_FOUND` (404) - Resource not found
- `CONFLICT` (409) - Duplicate entry or conflict
- `UNPROCESSABLE_ENTITY` (422) - Validation failed
- `INTERNAL_SERVER_ERROR` (500) - Server error

---

## Pagination

Endpoints that return lists support pagination:

```
?page=1&limit=20
```

Response includes:

```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "pages": 8
  }
}
```

---

## Rate Limiting

API endpoints are rate limited:
- **15 requests per minute** for authenticated users
- **5 requests per minute** for unauthenticated endpoints

Headers:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 99
X-RateLimit-Reset: 1626260400
```

---

For more information, visit: [API Documentation](API.md)
