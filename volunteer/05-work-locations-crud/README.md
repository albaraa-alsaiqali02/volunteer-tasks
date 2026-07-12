# Task 05 — Work Locations CRUD

> This task is the **pattern** you'll repeat in the Tasks and Volunteers tasks. Focus on it well.

## 🎯 Goal
Build a full CRUD for work locations (add / view / edit / delete).

## 📋 Requirements

### 1. The Controller
```bash
php artisan make:controller WorkLocationController --api
```
Define: `index`, `store`, `show`, `update`, `destroy`.

### 2. Validation (Form Requests)
```bash
php artisan make:request StoreWorkLocationRequest
php artisan make:request UpdateWorkLocationRequest
```
- Add validation rules (e.g., `name` required|string|max:255).
- Use them inside the Controller instead of writing validation in the methods.

### 3. API Resource
```bash
php artisan make:resource WorkLocationResource
```
- Define the JSON shape returned to the client (don't return the raw model).

### 4. Routes
In `routes/api.php`, inside the `auth:sanctum` group:
```php
Route::apiResource('work-locations', WorkLocationController::class);
```

### 5. Resulting endpoints
| Method | Endpoint | Function |
|--------|----------|----------|
| GET | `/api/work-locations` | List all |
| POST | `/api/work-locations` | Create |
| GET | `/api/work-locations/{id}` | Show one |
| PUT/PATCH | `/api/work-locations/{id}` | Update |
| DELETE | `/api/work-locations/{id}` | Delete |

## ✅ Deliverables
- 5 working endpoints, protected by authentication.
- Validation works and returns clear errors.
- Responses go through a Resource.

## 💡 Tips
- Use Eloquent: `WorkLocation::create()`, `->update()`, `->delete()`.
- Use **Route Model Binding** (pass `WorkLocation $workLocation` instead of `$id`).
- Return correct status codes (201 for create, 404 for not found…).

## 🔍 Acceptance Criteria
- [ ] All CRUD works
- [ ] Form Requests used
- [ ] Resource used
- [ ] Protected by Sanctum
