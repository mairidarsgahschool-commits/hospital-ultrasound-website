# Database Schema Documentation

## Overview

The Hospital Ultrasound Report Generator uses PostgreSQL as its primary database. This document describes the database schema, relationships, and design decisions.

## Database Name

```
hospital_ultrasound
```

## Tables

### Users Table

Stores information about all system users (doctors, radiologists, patients, staff, admins).

**Table Name:** `users`

**Columns:**
- `id` (UUID, PRIMARY KEY) - Unique user identifier
- `email` (VARCHAR 255, UNIQUE, NOT NULL) - Email address with format validation
- `password_hash` (VARCHAR 255, NOT NULL) - Bcrypt hashed password
- `first_name` (VARCHAR 100, NOT NULL) - User's first name
- `last_name` (VARCHAR 100, NOT NULL) - User's last name
- `phone` (VARCHAR 20) - Phone number
- `role` (VARCHAR 50, NOT NULL) - User role: admin, doctor, radiologist, patient, staff
- `is_active` (BOOLEAN, DEFAULT true) - Account status
- `email_verified` (BOOLEAN, DEFAULT false) - Email verification status
- `last_login` (TIMESTAMP) - Last login timestamp
- `created_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP) - Account creation time
- `updated_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP) - Last update time
- `deleted_at` (TIMESTAMP, NULLABLE) - Soft delete timestamp

**Indexes:**
- `idx_users_email` - Fast email lookups
- `idx_users_role` - Fast role-based queries
- `idx_users_active` - Fast active user queries

### Patients Table

Stores detailed patient medical and demographic information.

**Table Name:** `patients`

**Columns:**
- `id` (UUID, PRIMARY KEY) - Unique patient identifier
- `user_id` (UUID, FOREIGN KEY) - Reference to users table
- `medical_record_number` (VARCHAR 50, UNIQUE) - Hospital-assigned MRN
- `date_of_birth` (DATE) - Patient's DOB
- `gender` (VARCHAR 10) - male, female, other
- `blood_type` (VARCHAR 5) - Blood type (O+, A-, etc.)
- `address` (TEXT) - Street address
- `city` (VARCHAR 100) - City
- `state` (VARCHAR 100) - State/Province
- `postal_code` (VARCHAR 20) - Postal code
- `country` (VARCHAR 100) - Country
- `emergency_contact_name` (VARCHAR 100) - Emergency contact person
- `emergency_contact_phone` (VARCHAR 20) - Emergency contact phone
- `allergies` (TEXT) - Known allergies
- `medical_history` (TEXT) - Medical history notes
- `insurance_provider` (VARCHAR 100) - Insurance company name
- `insurance_policy_number` (VARCHAR 50) - Insurance policy ID
- `created_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP)
- `updated_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP)
- `deleted_at` (TIMESTAMP, NULLABLE)

**Indexes:**
- `idx_patients_mrn` - Fast MRN lookups
- `idx_patients_user_id` - Link to user accounts

### Departments Table

Organizational structure of hospital departments.

**Table Name:** `departments`

**Columns:**
- `id` (UUID, PRIMARY KEY)
- `name` (VARCHAR 100, UNIQUE) - Department name
- `description` (TEXT) - Department description
- `head_id` (UUID, FOREIGN KEY) - Department head (user ID)
- `is_active` (BOOLEAN, DEFAULT true) - Active status
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### User Departments (Many-to-Many)

Maps users to departments they work in.

**Table Name:** `user_departments`

**Columns:**
- `id` (UUID, PRIMARY KEY)
- `user_id` (UUID, FOREIGN KEY)
- `department_id` (UUID, FOREIGN KEY)
- `created_at` (TIMESTAMP)

### Ultrasound Images Table

Stores metadata about uploaded ultrasound images.

**Table Name:** `ultrasound_images`

**Columns:**
- `id` (UUID, PRIMARY KEY)
- `patient_id` (UUID, FOREIGN KEY) - Reference to patient
- `uploaded_by_id` (UUID, FOREIGN KEY) - Doctor/technician who uploaded
- `file_name` (VARCHAR 255) - Original filename
- `file_path` (VARCHAR 500) - Storage path
- `file_size` (BIGINT) - File size in bytes
- `mime_type` (VARCHAR 100) - MIME type (image/jpeg, etc.)
- `image_type` (VARCHAR 50) - Type: abdomen, cardiac, obstetric, vascular, thyroid, breast, other
- `dicom_data` (BOOLEAN, DEFAULT false) - DICOM format flag
- `description` (TEXT) - Image description
- `upload_date` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)
- `deleted_at` (TIMESTAMP, NULLABLE)

**Indexes:**
- `idx_ultrasound_patient_id` - Fast patient image lookups
- `idx_ultrasound_uploaded_by` - Fast uploader lookups
- `idx_ultrasound_image_type` - Filter by image type

### Ultrasound Reports Table

Stores generated ultrasound reports.

**Table Name:** `ultrasound_reports`

**Columns:**
- `id` (UUID, PRIMARY KEY)
- `patient_id` (UUID, FOREIGN KEY) - Patient reference
- `ultrasound_image_id` (UUID, FOREIGN KEY) - Image reference
- `created_by_id` (UUID, FOREIGN KEY) - Doctor who created report
- `reviewed_by_id` (UUID, FOREIGN KEY, NULLABLE) - Doctor who reviewed
- `report_date` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP) - Report creation date
- `exam_type` (VARCHAR 100) - Type of examination
- `clinical_indication` (TEXT) - Reason for examination
- `findings` (TEXT) - Clinical findings
- `impression` (TEXT) - Doctor's impression
- `recommendations` (TEXT) - Follow-up recommendations
- `status` (VARCHAR 50) - draft, pending_review, reviewed, final, archived
- `is_normal` (BOOLEAN) - Normal/abnormal flag
- `follow_up_required` (BOOLEAN) - Follow-up needed
- `follow_up_date` (DATE) - Recommended follow-up date
- `pdf_path` (VARCHAR 500) - Path to generated PDF
- `reviewed_at` (TIMESTAMP, NULLABLE) - Review timestamp
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)
- `deleted_at` (TIMESTAMP, NULLABLE)

**Indexes:**
- `idx_reports_patient_id` - Patient reports
- `idx_reports_created_by` - Doctor's reports
- `idx_reports_reviewed_by` - Reviewed reports
- `idx_reports_status` - Report status filtering
- `idx_reports_date` - Date range queries

### Report Templates Table

Templates for standard report formats.

**Table Name:** `report_templates`

**Columns:**
- `id` (UUID, PRIMARY KEY)
- `name` (VARCHAR 100, UNIQUE) - Template name
- `description` (TEXT)
- `exam_type` (VARCHAR 100) - Associated exam type
- `template_content` (TEXT) - HTML/Markdown template
- `created_by_id` (UUID, FOREIGN KEY) - Creator
- `is_active` (BOOLEAN, DEFAULT true)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### Audit Logs Table

Tracks all user actions for compliance and security.

**Table Name:** `audit_logs`

**Columns:**
- `id` (UUID, PRIMARY KEY)
- `user_id` (UUID, FOREIGN KEY, NULLABLE)
- `action` (VARCHAR 100) - Action performed (CREATE, UPDATE, DELETE, etc.)
- `entity_type` (VARCHAR 100) - Type of entity affected
- `entity_id` (VARCHAR 255) - ID of affected entity
- `changes` (JSONB) - Before/after changes
- `ip_address` (VARCHAR 45) - User's IP address
- `user_agent` (TEXT) - User's browser info
- `created_at` (TIMESTAMP)

**Indexes:**
- `idx_audit_user_id` - User activity
- `idx_audit_entity` - Entity change tracking
- `idx_audit_created_at` - Timeline queries

### Notifications Table

User notifications.

**Table Name:** `notifications`

**Columns:**
- `id` (UUID, PRIMARY KEY)
- `user_id` (UUID, FOREIGN KEY)
- `type` (VARCHAR 50) - Notification type
- `title` (VARCHAR 255)
- `message` (TEXT)
- `related_entity_type` (VARCHAR 100)
- `related_entity_id` (VARCHAR 255)
- `is_read` (BOOLEAN, DEFAULT false)
- `read_at` (TIMESTAMP, NULLABLE)
- `created_at` (TIMESTAMP)

### Sessions Table

Manages user sessions and tokens.

**Table Name:** `sessions`

**Columns:**
- `id` (UUID, PRIMARY KEY)
- `user_id` (UUID, FOREIGN KEY)
- `token` (VARCHAR 500, UNIQUE)
- `ip_address` (VARCHAR 45)
- `user_agent` (TEXT)
- `expires_at` (TIMESTAMP)
- `created_at` (TIMESTAMP)

### Password Reset Tokens Table

Manages password reset functionality.

**Table Name:** `password_reset_tokens`

**Columns:**
- `id` (UUID, PRIMARY KEY)
- `user_id` (UUID, FOREIGN KEY)
- `token` (VARCHAR 500, UNIQUE)
- `expires_at` (TIMESTAMP)
- `used_at` (TIMESTAMP, NULLABLE)
- `created_at` (TIMESTAMP)

## Relationships

### One-to-Many Relationships

1. **users → patients** - One user can have one patient profile
2. **users → sessions** - One user can have multiple sessions
3. **users → audit_logs** - One user can have many audit log entries
4. **users → notifications** - One user receives many notifications
5. **users → ultrasound_images** - One uploader creates multiple images
6. **users → ultrasound_reports** - One doctor creates/reviews multiple reports
7. **patients → ultrasound_images** - One patient has multiple images
8. **patients → ultrasound_reports** - One patient has multiple reports
9. **ultrasound_images → ultrasound_reports** - One image can have one or multiple reports
10. **departments → users** - One department has many users (through user_departments)

### Many-to-Many Relationships

1. **users ↔ departments** - Users can work in multiple departments
   - Join table: `user_departments`

## Triggers

### Updated At Triggers

Automatic timestamp updates on row modifications:

```sql
update_users_updated_at
update_departments_updated_at
update_patients_updated_at
update_ultrasound_images_updated_at
update_ultrasound_reports_updated_at
update_report_templates_updated_at
```

## Query Examples

### Get Patient with All Reports

```sql
SELECT p.*, u.first_name, u.last_name
FROM patients p
JOIN users u ON p.user_id = u.id
WHERE p.id = 'patient-uuid'
LIMIT 1;

SELECT ur.*, i.file_name
FROM ultrasound_reports ur
JOIN ultrasound_images i ON ur.ultrasound_image_id = i.id
WHERE ur.patient_id = 'patient-uuid'
ORDER BY ur.report_date DESC;
```

### Get Doctor's Recent Reports

```sql
SELECT ur.*, p.first_name, p.last_name, u.first_name as doctor_name
FROM ultrasound_reports ur
JOIN patients p ON ur.patient_id = p.id
JOIN ultrasound_images i ON ur.ultrasound_image_id = i.id
JOIN users u ON ur.created_by_id = u.id
WHERE ur.created_by_id = 'doctor-uuid'
AND ur.report_date > now() - INTERVAL '7 days'
ORDER BY ur.report_date DESC;
```

### Audit Trail for Entity

```sql
SELECT al.*, u.first_name, u.last_name
FROM audit_logs al
LEFT JOIN users u ON al.user_id = u.id
WHERE al.entity_type = 'ultrasound_reports'
AND al.entity_id = 'report-uuid'
ORDER BY al.created_at DESC;
```

## Backups

### PostgreSQL Backup

```bash
# Full backup
pg_dump -h localhost -U postgres -d hospital_ultrasound > backup.sql

# Restore
psql -h localhost -U postgres -d hospital_ultrasound < backup.sql

# Custom format backup (faster)
pg_dump -h localhost -U postgres -F c -d hospital_ultrasound > backup.dump
```

## Performance Considerations

1. **Indexes** - Ensure frequent queries have appropriate indexes
2. **Partitioning** - Consider partitioning large tables by date
3. **Archiving** - Move old reports to archive table
4. **Soft Deletes** - Using `deleted_at` field instead of hard deletes
5. **JSONB** - Using for flexible audit data

---

**Last Updated:** 2026-07-13
