# Task 07 — Volunteers CRUD

## 🎯 Goal
Build a full CRUD for volunteers.

## 📋 Requirements
Same pattern as [Task 05](../05-work-locations-crud):

1. `VolunteerController` (`--api`).
2. Form Requests: `StoreVolunteerRequest`, `UpdateVolunteerRequest`.
3. `VolunteerResource`.
4. Route: `Route::apiResource('volunteers', VolunteerController::class);` inside the `auth:sanctum` group.

### Endpoints
| Method | Endpoint |
|--------|----------|
| GET | `/api/volunteers` |
| POST | `/api/volunteers` |
| GET | `/api/volunteers/{id}` |
| PUT/PATCH | `/api/volunteers/{id}` |
| DELETE | `/api/volunteers/{id}` |

## ✅ Deliverables
- Full, working, protected CRUD for volunteers.

## 💡 Tips
- In `index`, consider showing each volunteer with their assignments (`with('assignments')`) — useful for the next task.
- Validate email/phone format in the Form Request.

## 🔍 Acceptance Criteria
- [ ] CRUD works
- [ ] Validation + Resource used
- [ ] Protected by Sanctum
