# Volunteer Coordination — Laravel Back-End Training Project

A training project for the **Back-End (Laravel)** track of the student training program.
The goal is to build a small **web application (API)** to coordinate volunteers and assign them to tasks across work locations.

> Read [`PROJECT.md`](./PROJECT.md) first — it contains the full project description and requirements.

---

## Project Idea (in short)

A platform managed by a **single user (Admin)** who is added manually to the database.
The Admin can:

- Log in / log out.
- Manage **work locations** (e.g., Al-Shifa Hospital, aid distribution center).
- Manage **tasks** (e.g., distribution, management, first aid, monitoring).
- Manage **volunteers**.
- **Assign a volunteer** to a work location, choosing their task at that location.

---

## How to work through the repo

The project is split into **progressive tasks**. Each task builds on the previous one.
Start from the first task and go in order — every folder has a `README.md` explaining what to do in detail.

| # | Task | Topic |
|---|------|-------|
| 01 | [Setup](./01-setup) | Install Laravel + environment & Git |
| 02 | [Database Design](./02-database-design) | ERD + Migrations |
| 03 | [Models & Relationships](./03-models-relationships) | Eloquent Models & Relationships |
| 04 | [Authentication](./04-authentication) | Login / Logout (Sanctum) |
| 05 | [Work Locations CRUD](./05-work-locations-crud) | Work Locations endpoints |
| 06 | [Tasks CRUD](./06-tasks-crud) | Tasks endpoints |
| 07 | [Volunteers CRUD](./07-volunteers-crud) | Volunteers endpoints |
| 08 | [Assignment](./08-assignment) | Assignment logic |
| 09 | [Finalize & Standards](./09-finalize-standards) | Validation, Resources, Standards |

The [`resources`](./resources) folder has links and references to help you.

---

## Submission

- Each student **forks** the repo, or works on a dedicated **branch** named after them.
- Make a **clear commit per task** (not one big commit at the end).
- Use clear commit messages, e.g.: `task-04: add login & logout endpoints`.
- After each task, open a **Pull Request** for review.

---

## General Rules (important)

1. **Organize files** properly with clear standards (Controllers, Models, Requests, Resources, Routes).
2. Use **Eloquent** and **Eloquent Relationships** for all database operations — no raw Query Builder or direct SQL unless absolutely necessary.
3. **Validate** every input (use Form Requests).
4. API responses must be **consistent JSON** (use API Resources).
5. Clear names for routes, methods, and variables.
6. Never commit `.env` or `vendor/` (make sure `.gitignore` is set).

---

## Technical Requirements

- PHP 8.2+
- Laravel 11/12
- Database: MySQL or PostgreSQL
- Laravel Sanctum (token-based authentication)
- API testing tool: Postman / Insomnia

---

## Evaluation Criteria (general checklist)

- [ ] Project structure is organized with clear standards
- [ ] Relationships are defined and correct
- [ ] All CRUD endpoints work
- [ ] Authentication works and protects routes
- [ ] Assignment logic is correct (volunteer + work location + task)
- [ ] Validation is present and sufficient
- [ ] Responses are clean JSON (Resources)
- [ ] Clean Git history with a clear commit per task
