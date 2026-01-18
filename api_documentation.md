# Student Record Management System - API Documentation

## Base URL
```
http://localhost:3000/api
```

## Authentication
Most endpoints require a JWT token in the Authorization header:
```
Authorization: Bearer <token>
```

---

## Authentication Endpoints

### 1. Login
**POST** `/auth/login`

**Request Body:**
```json
{
  "userType": "staff|student|parent",
  "username": "ADMIN or ROLL_NUMBER or parent@XXX",
  "password": "password"
}
```

**Response:**
```json
{
  "token": "jwt_token",
  "user": {
    "username": "ADMIN",
    "role": "staff"
  }
}
```

### 2. Change Password
**POST** `/auth/change-password`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "newPassword": "newpass123",
  "confirmPassword": "newpass123"
}
```

**Response:**
```json
{
  "message": "Password updated successfully"
}
```

### 3. Forgot Password Request
**POST** `/auth/forgot-password`

**Request Body:**
```json
{
  "userType": "student|parent",
  "username": "ROLL_NUMBER or parent@XXX"
}
```

**Response:**
```json
{
  "message": "Password reset request submitted",
  "status": "requested"
}
```

### 4. Approve Password Reset (Staff Only)
**POST** `/auth/approve-reset/:roll`

**Headers:** `Authorization: Bearer <token>`

**Response:**
```json
{
  "message": "Password reset approved"
}
```

---

## Student Management Endpoints

### 1. Get All Students (Staff Only)
**GET** `/students`

**Headers:** `Authorization: Bearer <token>`

**Response:**
```json
[
  {
    "id": 1,
    "roll": "111625104001",
    "name": "KISHORE",
    "dob": "2005-01-15",
    "gender": "Male",
    "dept": "Computer Science",
    "year": "2",
    "current_semester": 3,
    "total_days": 100,
    "days_present": 90,
    "attendance_percentage": 90.00
  }
]
```

### 2. Get Single Student
**GET** `/students/:roll`

**Headers:** `Authorization: Bearer <token>`

**Response:**
```json
{
  "id": 1,
  "roll": "111625104001",
  "name": "KISHORE",
  "dob": "2005-01-15",
  "gender": "Male",
  "dept": "Computer Science",
  "year": "2",
  "current_semester": 3,
  "total_days": 100,
  "days_present": 90,
  "marks": {
    "sem1": {
      "int1": 85,
      "int2": 90,
      "model": 88,
      "semFinal": 92,
      "assignment": 9,
      "miniProject": 5,
      "rmkNextGen": 9
    }
  }
}
```

### 3. Create Student (Staff Only)
**POST** `/students`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "name": "KISHORE",
  "roll": "111625104001",
  "dob": "2005-01-15",
  "gender": "Male",
  "dept": "Computer Science",
  "year": "2",
  "currentSemester": 3
}
```

**Response:**
```json
{
  "message": "Student created successfully",
  "roll": "111625104001",
  "defaultPassword": "104@1234"
}
```

### 4. Update Student (Staff Only)
**PUT** `/students/:roll`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "name": "KISHORE UPDATED",
  "dob": "2005-01-15",
  "gender": "Male",
  "dept": "Computer Science",
  "year": "3",
  "currentSemester": 5
}
```

**Response:**
```json
{
  "message": "Student updated successfully"
}
```

### 5. Delete Student (Staff Only)
**DELETE** `/students/:roll`

**Headers:** `Authorization: Bearer <token>`

**Response:**
```json
{
  "message": "Student deleted successfully"
}
```

### 6. Get Password Reset Requests (Staff Only)
**GET** `/students/password-requests`

**Headers:** `Authorization: Bearer <token>`

**Response:**
```json
[
  {
    "roll": "111625104001",
    "name": "KISHORE"
  }
]
```

---

## Attendance Endpoints

### 1. Update Attendance (Staff Only)
**PUT** `/attendance/:roll`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "totalDays": 100,
  "daysPresent": 90
}
```

**Response:**
```json
{
  "message": "Attendance updated successfully"
}
```

---

## Marks Endpoints

### 1. Update Marks (Staff Only)
**PUT** `/marks/:roll/:semester`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "int1": 85,
  "int2": 90,
  "model": 88,
  "semFinal": 92,
  "assignment": 9,
  "miniProject": 5,
  "rmkNextGen": 9
}
```

**Response:**
```json
{
  "message": "Marks updated successfully"
}
```

---

## Content Management Endpoints

### 1. Upload Timetable (Staff Only)
**POST** `/timetable`

**Headers:** `Authorization: Bearer <token>`

**Content-Type:** `multipart/form-data`

**Form Data:**
- `semester`: "sem1" to "sem8"
- `timetable`: Image file (JPG/PNG)

**Response:**
```json
{
  "message": "Timetable uploaded successfully",
  "filePath": "/uploads/timetables/123456-timetable.jpg"
}
```

### 2. Upload Digital Content (Staff Only)
**POST** `/digital-content`

**Headers:** `Authorization: Bearer <token>`

**Content-Type:** `multipart/form-data`

**Form Data:**
- `semester`: "sem1" to "sem8"
- `title`: "Module 1 Notes"
- `content`: PDF/DOCX file (optional)
- `url`: "https://example.com/notes.pdf" (optional)

**Response:**
```json
{
  "message": "Content uploaded successfully",
  "filePath": "/uploads/content/123456-notes.pdf"
}
```

### 3. Get Timetable
**GET** `/timetable/:semester`

**Headers:** `Authorization: Bearer <token>`

**Response:**
```json
{
  "id": 1,
  "semester": "sem1",
  "file_path": "/uploads/timetables/123456-timetable.jpg",
  "uploaded_at": "2025-01-18T10:30:00.000Z"
}
```

### 4. Get Digital Content
**GET** `/digital-content/:semester`

**Headers:** `Authorization: Bearer <token>`

**Response:**
```json
[
  {
    "id": 1,
    "semester": "sem1",
    "title": "Module 1 Notes",
    "file_path": "/uploads/content/123456-notes.pdf",
    "url": null,
    "uploaded_at": "2025-01-18T10:30:00.000Z"
  }
]
```

---

## Error Responses

All endpoints may return the following error responses:

**400 Bad Request**
```json
{
  "error": "Error message describing what went wrong"
}
```

**401 Unauthorized**
```json
{
  "error": "Access token required"
}
```

**403 Forbidden**
```json
{
  "error": "Insufficient permissions"
}
```

**404 Not Found**
```json
{
  "error": "Resource not found"
}
```

**500 Internal Server Error**
```json
{
  "error": "Something went wrong!"
}
```

---

## Database Schema

### Students Table
- `id`: INTEGER PRIMARY KEY
- `roll`: TEXT UNIQUE NOT NULL
- `name`: TEXT NOT NULL
- `dob`: TEXT NOT NULL
- `gender`: TEXT NOT NULL
- `dept`: TEXT NOT NULL
- `year`: TEXT NOT NULL
- `current_semester`: INTEGER NOT NULL
- `password`: TEXT NOT NULL
- `forgot_password_requested`: TEXT DEFAULT 'false'
- `created_at`: DATETIME
- `updated_at`: DATETIME

### Attendance Table
- `id`: INTEGER PRIMARY KEY
- `student_roll`: TEXT (Foreign Key)
- `total_days`: INTEGER DEFAULT 0
- `days_present`: INTEGER DEFAULT 0
- `updated_at`: DATETIME

### Marks Table
- `id`: INTEGER PRIMARY KEY
- `student_roll`: TEXT (Foreign Key)
- `semester`: TEXT NOT NULL
- `int1, int2, model`: INTEGER
- `sem_final`: INTEGER
- `assignment, mini_project, rmk_next_gen`: INTEGER
- `updated_at`: DATETIME
- UNIQUE(student_roll, semester)

### Timetables Table
- `id`: INTEGER PRIMARY KEY
- `semester`: TEXT UNIQUE NOT NULL
- `file_path`: TEXT NOT NULL
- `uploaded_at`: DATETIME

### Digital Content Table
- `id`: INTEGER PRIMARY KEY
- `semester`: TEXT NOT NULL
- `title`: TEXT NOT NULL
- `file_path`: TEXT
- `url`: TEXT
- `uploaded_at`: DATETIME