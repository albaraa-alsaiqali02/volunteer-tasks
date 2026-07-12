# Volunteer Platform — Training Plan

A step-by-step breakdown of the project into successive training sessions.
Each session builds on the previous one. Students build the project from scratch.

---

## Project Overview

A volunteer coordination system where an admin can manage:
- **Work Locations** — places where volunteers are deployed
- **Tasks** — jobs to be done at those locations
- **Volunteers** — people available to work
- **Assignments** — linking a volunteer to a location and task

**Stack:** Laravel 11 · Laravel Sanctum · Blade · AdminLTE 3 · Vanilla JS (Fetch API)

---

## Session 1 — Laravel Fundamentals & Project Setup

**Goal:** Understand what Laravel is and get the project running locally.

**Topics:**
- What is a framework and why use one?
- MVC pattern (Model · View · Controller)
- Laravel directory structure walkthrough
- Artisan CLI basics (`php artisan list`, `serve`, `about`)
- `.env` file and environment configuration
- Connecting to a database (MySQL/SQLite)

**Hands-on:**
```bash
composer create-project laravel/laravel volunteer-platform
cd volunteer-platform
cp .env.example .env
php artisan key:generate
php artisan serve
```

**Files to explore:**
- `app/`, `routes/`, `resources/`, `database/`, `config/`

**Outcome:** A running Laravel app in the browser.

---

## Session 2 — Database Design & Migrations

**Goal:** Design the database schema and create it using Laravel migrations.

**Topics:**
- Relational database design (tables, foreign keys)
- What are migrations and why use them instead of raw SQL?
- `Schema::create()`, column types, constraints
- `php artisan make:migration`, `migrate`, `migrate:rollback`
- Seeders and `php artisan db:seed`

**Tables to build (in order):**

| Table | Key columns |
|-------|-------------|
| `users` | id, name, email, password |
| `work_locations` | id, name, type, address, description |
| `tasks` | id, name, description |
| `volunteers` | id, name, email, phone, national_id, address |
| `assignments` | id, volunteer_id, work_location_id, task_id, start_date, end_date, notes |

**Files:**
- `database/migrations/`
- `database/seeders/AdminSeeder.php`

**Outcome:** A fully structured database with a seeded admin user.

---

## Session 3 — Eloquent Models & Relationships

**Goal:** Understand ORM and define how models relate to each other.

**Topics:**
- What is an ORM? Eloquent basics
- `$fillable` and mass assignment protection
- `$casts` for dates and types
- Relationships: `hasMany`, `belongsTo`
- Eager loading with `with()` and `load()`
- Tinker (`php artisan tinker`) for live model testing

**Models to build:**

```
WorkLocation  ──has many──▶  Assignment
Task          ──has many──▶  Assignment
Volunteer     ──has many──▶  Assignment
Assignment    ──belongs to──▶  Volunteer, WorkLocation, Task
```

**Files:**
- `app/Models/WorkLocation.php`
- `app/Models/Task.php`
- `app/Models/Volunteer.php`
- `app/Models/Assignment.php`

**Outcome:** Models that can query related data with a single method call.

---

## Session 4 — Authentication with Laravel Sanctum

**Goal:** Protect the API with token-based authentication.

**Topics:**
- How API authentication works (tokens vs sessions)
- Installing and configuring Laravel Sanctum
- `HasApiTokens` trait on the User model
- `createToken()` and `plainTextToken`
- `auth:sanctum` middleware
- Login, logout, and "me" endpoints

**Endpoints to build:**

| Method | URI | Description |
|--------|-----|-------------|
| POST | `/api/login` | Returns a bearer token |
| POST | `/api/logout` | Revokes current token |
| GET | `/api/me` | Returns authenticated user |

**Files:**
- `app/Http/Controllers/Api/AuthController.php`
- `routes/api.php`

**Test with Postman or curl:**
```bash
curl -X POST http://localhost:8000/api/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@example.com","password":"password"}'
```

**Outcome:** A working login system that issues API tokens.

---

## Session 5 — API Controllers (CRUD)

**Goal:** Build RESTful API endpoints for all four resources.

**Topics:**
- REST conventions (GET / POST / PUT / DELETE)
- `apiResource` route shortcut
- Request validation with `$request->validate()`
- JSON responses with `response()->json()`
- Route model binding
- HTTP status codes (200, 201, 404, 422)

**Controllers to build:**

| Controller | Resource | Notable features |
|------------|----------|-----------------|
| `WorkLocationController` | `/api/work-locations` | `withCount('assignments')` on index |
| `TaskController` | `/api/tasks` | Simple CRUD |
| `VolunteerController` | `/api/volunteers` | `unique` email & national_id validation |
| `AssignmentController` | `/api/assignments` | `with(['volunteer','workLocation','task'])` on index |

**Files:**
- `app/Http/Controllers/Api/` (all four controllers)
- `routes/api.php`

**Outcome:** A fully functional REST API testable in Postman.

---

## Session 6 — Blade Layouts & AdminLTE

**Goal:** Build the application shell (sidebar, navbar, layout).

**Topics:**
- How Blade templating works
- `@extends`, `@section`, `@yield`
- Integrating a CSS admin template (AdminLTE via CDN)
- Active nav-link highlighting with `request()->routeIs()`
- Storing the API token in `localStorage`
- Global `apiHeaders()` JS helper
- Auth guard: redirect to `/login` if no token

**Files:**
- `resources/views/layouts/app.blade.php`
- `routes/web.php` (the six view-only routes)

**Outcome:** A styled shell that all pages will extend, with working sidebar navigation.

---

## Session 7 — Login Page & JS Authentication

**Goal:** Build the login page that exchanges credentials for an API token.

**Topics:**
- Blade views without extending the layout (standalone page)
- Using `fetch()` to call the API from the browser
- Storing the token: `localStorage.setItem('api_token', token)`
- Redirecting with `window.location.href`
- SweetAlert2 for user-friendly error messages

**File:**
- `resources/views/pages/login.blade.php`

**Flow to implement:**
1. User submits email + password form
2. JS posts to `POST /api/login`
3. On success → save token → redirect to `/dashboard`
4. On failure → show error message

**Outcome:** A working login flow that gates the rest of the app.

---

## Session 8 — Dashboard Page

**Goal:** Build a live dashboard that fetches stats from the API.

**Topics:**
- `Promise.all()` for parallel API calls
- Updating the DOM dynamically with `.textContent`
- Rendering a table from a JSON array
- Optional chaining (`a.volunteer?.name ?? '—'`)
- `Array.prototype.slice()` to show only the latest 10 records

**File:**
- `resources/views/pages/dashboard.blade.php`

**Stats cards to populate:**

| Card | API call |
|------|----------|
| Work Locations | `GET /api/work-locations` |
| Tasks | `GET /api/tasks` |
| Volunteers | `GET /api/volunteers` |
| Assignments | `GET /api/assignments` |

**Outcome:** A dashboard that shows live counts and the 10 most recent assignments.

---

## Session 9 — Work Locations & Tasks Pages

**Goal:** Build the first two full CRUD pages.

**Topics:**
- Fetching a list and rendering it into a table
- Modal forms (AdminLTE + Bootstrap modals)
- `POST` / `PUT` / `DELETE` with `fetch()`
- Edit: pre-filling modal fields from the selected row's data
- Delete: confirmation dialog with SweetAlert2
- Resetting forms after submit

**Files:**
- `resources/views/pages/work-locations.blade.php`
- `resources/views/pages/tasks.blade.php`

**CRUD actions per page:**

| Action | Method | Endpoint |
|--------|--------|----------|
| Load list | GET | `/api/work-locations` |
| Create | POST | `/api/work-locations` |
| Edit | PUT | `/api/work-locations/{id}` |
| Delete | DELETE | `/api/work-locations/{id}` |

**Outcome:** Two fully interactive CRUD pages without any page reload.

---

## Session 10 — Volunteers & Assignments Pages

**Goal:** Complete the remaining two pages, including the relational Assignments form.

**Topics:**
- Populating `<select>` dropdowns from API data (volunteers, locations, tasks)
- Handling related data in edit mode
- Date inputs (`start_date`, `end_date`)
- Displaying related model names in a table (`a.volunteer.name`)
- Handling cascade deletes (volunteer deleted → assignments gone)

**Files:**
- `resources/views/pages/volunteers.blade.php`
- `resources/views/pages/assignments.blade.php`

**Assignments form fields:**
- Volunteer (dropdown from `/api/volunteers`)
- Work Location (dropdown from `/api/work-locations`)
- Task (dropdown from `/api/tasks`)
- Start Date / End Date
- Notes (textarea)

**Outcome:** The project is complete and fully functional end-to-end.

---

## Summary Table

| # | Session | Key Concept |
|---|---------|-------------|
| 1 | Setup & Structure | Laravel MVC, Artisan, `.env` |
| 2 | Database & Migrations | Schema design, `migrate`, seeders |
| 3 | Models & Relationships | Eloquent ORM, `hasMany`, `belongsTo` |
| 4 | Sanctum Auth | Token-based API authentication |
| 5 | API Controllers | REST, validation, JSON responses |
| 6 | Blade Layout | Templates, AdminLTE, global JS helpers |
| 7 | Login Page | `fetch()`, `localStorage`, redirect |
| 8 | Dashboard | `Promise.all()`, dynamic DOM |
| 9 | Work Locations & Tasks | Full CRUD with modals |
| 10 | Volunteers & Assignments | Relational dropdowns, complete app |
