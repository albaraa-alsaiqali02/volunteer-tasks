# Software Requirements Specification (SRS)
## Volunteer Coordination Platform — REST API

**Version:** 1.0  
**Date:** 2026-06-21  
**Prepared for:** Training Students  

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Overall Description](#2-overall-description)
3. [System Architecture](#3-system-architecture)
4. [Database Design](#4-database-design)
5. [Functional Requirements](#5-functional-requirements)
6. [API Specification](#6-api-specification)
7. [Non-Functional Requirements](#7-non-functional-requirements)
8. [Technology Stack](#8-technology-stack)

---

## 1. Introduction

### 1.1 Purpose

This document describes the software requirements for the **Volunteer Coordination Platform** — a RESTful API that allows an administrator to manage volunteers, work locations, tasks, and assignments in a centralized system.

This SRS serves as the reference document for students building the project during training sessions.

### 1.2 Scope

The system exposes a JSON REST API that enables a single admin to:

- Register and manage volunteers.
- Define work locations and tasks.
- Assign volunteers to specific locations and tasks.
- Retrieve summary statistics and recent activity.

The system is a **backend-only** project. There is no frontend, no HTML, and no browser UI. All interaction happens through API clients such as Postman or curl.

### 1.3 Definitions

| Term | Meaning |
|------|---------|
| Admin | The single authorized user who manages the system |
| Volunteer | A person registered in the system who can be assigned to work |
| Work Location | A physical place where volunteers are deployed |
| Task | A job or role to be performed at a work location |
| Assignment | A record that links a volunteer to a work location and a task for a date range |
| Token | A secure string issued on login, sent with every subsequent API request |
| Bearer Token | An authorization header value in the format `Authorization: Bearer <token>` |
| Endpoint | A URL that the API exposes to perform a specific action |
| Resource | A named entity the API manages (e.g. volunteers, tasks) |

### 1.4 Overview

The application is a single Laravel 11 project that:

1. Accepts HTTP requests from any client (Postman, mobile app, frontend, etc.).
2. Validates the request and checks authentication.
3. Performs the requested database operation via Eloquent ORM.
4. Returns a structured JSON response.

---

## 2. Overall Description

### 2.1 Product Perspective

```
API Client (Postman / curl / any frontend)
       │
       │  HTTP Requests — JSON body / JSON response
       ▼
Laravel 11 REST API (routes/api.php)
       │
       │  Eloquent ORM
       ▼
MySQL / SQLite Database
```

### 2.2 User Classes

| User | Description |
|------|-------------|
| Admin | The only user of the system. Has full access to all endpoints. Authenticated via email and password, receives a bearer token. |

There are no roles, no public users, and no self-registration.

### 2.3 Constraints

- Only one admin account exists, created via a database seeder (`AdminSeeder`).
- All API endpoints except `POST /api/login` require a valid bearer token in the `Authorization` header.
- Tokens are issued by Laravel Sanctum and stored in the `personal_access_tokens` table.
- A token is revoked when the admin calls `POST /api/logout`.

---

## 3. System Architecture

### 3.1 MVC Pattern (API-focused)

| Layer | Role | Location |
|-------|------|----------|
| **Model** | Represents database tables and their relationships | `app/Models/` |
| **Controller** | Handles HTTP requests, validates input, returns JSON | `app/Http/Controllers/Api/` |
| **Routes** | Maps HTTP methods + URIs to controller methods | `routes/api.php` |

> There are no Views. The `resources/views/` folder is not used.

### 3.2 Request Lifecycle

```
HTTP Request
    │
    ▼
routes/api.php  →  Middleware (auth:sanctum)  →  Controller  →  Model / DB  →  JSON Response
```

### 3.3 Directory Structure (Key Folders)

```
volunteer-platform/
├── app/
│   ├── Http/Controllers/Api/    ← All API controllers
│   └── Models/                  ← Eloquent models
├── database/
│   ├── migrations/              ← Table definitions
│   └── seeders/AdminSeeder.php  ← Seeds the admin user
└── routes/
    └── api.php                  ← All API routes
```

---

## 4. Database Design

### 4.1 Entity-Relationship Summary

```
work_locations ──────────────────────┐
                                     ▼
tasks ───────────────────────▶  assignments ◀─── volunteers
```

Each **assignment** belongs to exactly one volunteer, one work location, and one task.

### 4.2 Tables

#### `users`
| Column | Type | Notes |
|--------|------|-------|
| id | BIGINT (PK) | Auto-increment |
| name | VARCHAR | Admin's display name |
| email | VARCHAR | Unique. Used to log in |
| password | VARCHAR | Bcrypt-hashed |
| created_at / updated_at | TIMESTAMP | Auto-managed by Laravel |

#### `personal_access_tokens`
Managed automatically by Laravel Sanctum. Stores issued tokens.

#### `work_locations`
| Column | Type | Notes |
|--------|------|-------|
| id | BIGINT (PK) | |
| name | VARCHAR | Name of the location |
| type | VARCHAR | e.g. hospital, school, shelter |
| address | VARCHAR | Physical address |
| description | TEXT | Nullable |
| created_at / updated_at | TIMESTAMP | |

#### `tasks`
| Column | Type | Notes |
|--------|------|-------|
| id | BIGINT (PK) | |
| name | VARCHAR | Task title |
| description | TEXT | Nullable |
| created_at / updated_at | TIMESTAMP | |

#### `volunteers`
| Column | Type | Notes |
|--------|------|-------|
| id | BIGINT (PK) | |
| name | VARCHAR | |
| email | VARCHAR | Unique |
| phone | VARCHAR | |
| national_id | VARCHAR | Unique |
| address | VARCHAR | |
| created_at / updated_at | TIMESTAMP | |

#### `assignments`
| Column | Type | Notes |
|--------|------|-------|
| id | BIGINT (PK) | |
| volunteer_id | BIGINT (FK) | References `volunteers.id` — cascade delete |
| work_location_id | BIGINT (FK) | References `work_locations.id` |
| task_id | BIGINT (FK) | References `tasks.id` |
| start_date | DATE | Required |
| end_date | DATE | Nullable |
| notes | TEXT | Nullable |
| created_at / updated_at | TIMESTAMP | |

### 4.3 Eloquent Relationships

| Model | Relationship | Target |
|-------|-------------|--------|
| `WorkLocation` | `hasMany` | `Assignment` |
| `Task` | `hasMany` | `Assignment` |
| `Volunteer` | `hasMany` | `Assignment` |
| `Assignment` | `belongsTo` | `Volunteer`, `WorkLocation`, `Task` |

---

## 5. Functional Requirements

### 5.1 Authentication

| ID | Requirement |
|----|-------------|
| AUTH-1 | The admin can authenticate by sending their email and password to `POST /api/login`. |
| AUTH-2 | On successful login, the API returns a bearer token and the user's basic information. |
| AUTH-3 | On failed login (wrong credentials), the API returns a `401` response with an error message. |
| AUTH-4 | The admin can log out by calling `POST /api/logout`, which revokes the current token. |
| AUTH-5 | All endpoints except `POST /api/login` return `401` if no valid token is provided. |
| AUTH-6 | The admin can retrieve their own profile via `GET /api/me`. |

### 5.2 Work Locations

| ID | Requirement |
|----|-------------|
| WL-1 | The admin can retrieve a list of all work locations. |
| WL-2 | Each work location in the list includes the count of assignments linked to it. |
| WL-3 | The admin can retrieve the details of a single work location by its ID. |
| WL-4 | The admin can create a new work location by providing name, type, and address (required), plus an optional description. |
| WL-5 | The admin can update an existing work location's fields. |
| WL-6 | The admin can delete a work location by its ID. |

### 5.3 Tasks

| ID | Requirement |
|----|-------------|
| TASK-1 | The admin can retrieve a list of all tasks. |
| TASK-2 | The admin can retrieve the details of a single task by its ID. |
| TASK-3 | The admin can create a new task by providing a name (required) and optional description. |
| TASK-4 | The admin can update an existing task. |
| TASK-5 | The admin can delete a task by its ID. |

### 5.4 Volunteers

| ID | Requirement |
|----|-------------|
| VOL-1 | The admin can retrieve a list of all volunteers. |
| VOL-2 | The admin can retrieve the details of a single volunteer by their ID. |
| VOL-3 | The admin can register a new volunteer with: name, email, phone, national_id, and address. |
| VOL-4 | The `email` and `national_id` fields must be unique across all volunteers. |
| VOL-5 | The admin can update an existing volunteer's information. |
| VOL-6 | The admin can delete a volunteer. All of the volunteer's assignments are deleted automatically. |

### 5.5 Assignments

| ID | Requirement |
|----|-------------|
| ASGN-1 | The admin can retrieve a list of all assignments. Each record includes the volunteer's name, work location name, and task name. |
| ASGN-2 | The admin can retrieve the details of a single assignment by its ID. |
| ASGN-3 | The admin can create a new assignment by providing: `volunteer_id`, `work_location_id`, `task_id`, and `start_date` (required), plus optional `end_date` and `notes`. |
| ASGN-4 | The provided `volunteer_id`, `work_location_id`, and `task_id` must reference existing records. |
| ASGN-5 | The admin can update an existing assignment. |
| ASGN-6 | The admin can delete an assignment by its ID. |

---

## 6. API Specification

### 6.1 Base URL

```
http://localhost:8000/api
```

### 6.2 Authentication Header (required on all endpoints except login)

```
Authorization: Bearer <token>
Content-Type: application/json
```

---

### 6.3 Auth Endpoints

#### `POST /api/login`
Authenticate and receive a token.

**Request body:**
```json
{
  "email": "admin@example.com",
  "password": "password"
}
```

**Success (200):**
```json
{
  "token": "1|abc123xyz...",
  "user": {
    "id": 1,
    "name": "Admin",
    "email": "admin@example.com"
  }
}
```

**Failure (401):**
```json
{
  "message": "Invalid credentials."
}
```

---

#### `POST /api/logout`
Revoke the current token. *(Requires auth)*

**Success (200):**
```json
{
  "message": "Logged out successfully."
}
```

---

#### `GET /api/me`
Return the authenticated admin's profile. *(Requires auth)*

**Success (200):**
```json
{
  "id": 1,
  "name": "Admin",
  "email": "admin@example.com"
}
```

---

### 6.4 Work Locations

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/work-locations` | List all work locations (with assignment count) |
| POST | `/api/work-locations` | Create a new work location |
| GET | `/api/work-locations/{id}` | Get a single work location |
| PUT | `/api/work-locations/{id}` | Update a work location |
| DELETE | `/api/work-locations/{id}` | Delete a work location |

**Required fields (create/update):** `name`, `type`, `address`  
**Optional fields:** `description`

**Example — GET /api/work-locations (200):**
```json
[
  {
    "id": 1,
    "name": "City Hospital",
    "type": "hospital",
    "address": "123 Main St",
    "description": null,
    "assignments_count": 4
  }
]
```

---

### 6.5 Tasks

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tasks` | List all tasks |
| POST | `/api/tasks` | Create a new task |
| GET | `/api/tasks/{id}` | Get a single task |
| PUT | `/api/tasks/{id}` | Update a task |
| DELETE | `/api/tasks/{id}` | Delete a task |

**Required fields (create/update):** `name`  
**Optional fields:** `description`

---

### 6.6 Volunteers

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/volunteers` | List all volunteers |
| POST | `/api/volunteers` | Register a new volunteer |
| GET | `/api/volunteers/{id}` | Get a single volunteer |
| PUT | `/api/volunteers/{id}` | Update a volunteer |
| DELETE | `/api/volunteers/{id}` | Delete a volunteer (cascades to assignments) |

**Required fields (create/update):** `name`, `email`, `phone`, `national_id`  
**Optional fields:** `address`  
**Unique fields:** `email`, `national_id`

---

### 6.7 Assignments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/assignments` | List all assignments (includes related names) |
| POST | `/api/assignments` | Create a new assignment |
| GET | `/api/assignments/{id}` | Get a single assignment |
| PUT | `/api/assignments/{id}` | Update an assignment |
| DELETE | `/api/assignments/{id}` | Delete an assignment |

**Required fields (create/update):** `volunteer_id`, `work_location_id`, `task_id`, `start_date`  
**Optional fields:** `end_date`, `notes`

**Example — GET /api/assignments (200):**
```json
[
  {
    "id": 1,
    "start_date": "2026-06-01",
    "end_date": "2026-06-30",
    "notes": "Morning shift only",
    "volunteer": { "id": 2, "name": "Sara Ahmed" },
    "work_location": { "id": 1, "name": "City Hospital" },
    "task": { "id": 3, "name": "Patient Support" }
  }
]
```

---

### 6.8 HTTP Status Codes

| Code | Meaning | When used |
|------|---------|-----------|
| 200 | OK | Successful GET, PUT, DELETE |
| 201 | Created | Successful POST (resource created) |
| 401 | Unauthorized | Missing or invalid token |
| 404 | Not Found | Resource ID does not exist |
| 422 | Unprocessable Entity | Validation failed (returns field errors) |

**Example — 422 Validation Error:**
```json
{
  "message": "The email has already been taken.",
  "errors": {
    "email": ["The email has already been taken."]
  }
}
```

---

## 7. Non-Functional Requirements

| ID | Category | Requirement |
|----|----------|-------------|
| NFR-1 | Security | All endpoints except `POST /api/login` are protected by `auth:sanctum` middleware. |
| NFR-2 | Security | Passwords are stored as bcrypt hashes. Plain-text passwords are never stored or returned. |
| NFR-3 | Security | Mass assignment is prevented via the `$fillable` property on all Eloquent models. |
| NFR-4 | Data Integrity | `email` and `national_id` are unique per volunteer and validated on create and update. |
| NFR-5 | Data Integrity | Deleting a volunteer automatically deletes all of their assignments (cascade delete). |
| NFR-6 | Consistency | All API responses are in JSON format with appropriate HTTP status codes. |
| NFR-7 | Consistency | All error responses follow the same structure with a `message` field. |

---

## 8. Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Backend framework | Laravel 11 | Routing, controllers, ORM, validation |
| Authentication | Laravel Sanctum | Token-based API authentication |
| Database | MySQL or SQLite | Data persistence |
| ORM | Eloquent (built into Laravel) | Database queries and relationships |
| CLI tool | Artisan (`php artisan`) | Migrations, seeders, local server |
| Package manager | Composer | PHP dependency management |
| API testing tool | Postman or curl | Testing and exploring the API during development |

---

*End of SRS — Volunteer Coordination Platform v1.0 (API Only)*
