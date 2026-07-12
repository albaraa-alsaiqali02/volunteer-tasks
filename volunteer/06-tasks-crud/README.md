# Task 06 — Tasks CRUD

## 🎯 Goal
Build a full CRUD for tasks (e.g., distribution, management, first aid, monitoring).

## 📋 Requirements
Repeat the same pattern from [Task 05](../05-work-locations-crud):

1. `TaskController` (`--api`).
2. Form Requests: `StoreTaskRequest`, `UpdateTaskRequest`.
3. `TaskResource`.
4. Route: `Route::apiResource('tasks', TaskController::class);` inside the `auth:sanctum` group.

### Endpoints
| Method | Endpoint |
|--------|----------|
| GET | `/api/tasks` |
| POST | `/api/tasks` |
| GET | `/api/tasks/{id}` |
| PUT/PATCH | `/api/tasks/{id}` |
| DELETE | `/api/tasks/{id}` |

## ✅ Deliverables
- Full, working, protected CRUD for tasks.

## 💡 Tips
- Don't copy code blindly — understand the pattern and apply it.
- Make `name` required and `unique` if it makes sense that task names shouldn't repeat.

## 🔍 Acceptance Criteria
- [ ] CRUD works
- [ ] Validation + Resource used
- [ ] Protected by Sanctum
