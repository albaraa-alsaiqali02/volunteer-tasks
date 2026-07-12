# Task 08 — Volunteer Assignment

> This is the heart of the project. The previous tasks were preparation for it.

## 🎯 Goal
Assign a volunteer to a **previously added work location**, choosing their **task** (from the previously added tasks) at that location.

## 📋 Requirements

### 1. The Controller
```bash
php artisan make:controller AssignmentController --api
```
At minimum: `index`, `store`, `destroy` (and optionally `update` to change the task/location).

### 2. Assignment logic (`store`)
The request must include:
- `volunteer_id`
- `work_location_id`
- `task_id`

### 3. Validation (very important)
In `StoreAssignmentRequest`:
- The three IDs must **actually exist** in the database → use `exists`:
  ```php
  'volunteer_id'      => 'required|exists:volunteers,id',
  'work_location_id'  => 'required|exists:work_locations,id',
  'task_id'           => 'required|exists:tasks,id',
  ```
- (Optional, advanced) Prevent duplicating the same assignment for the same volunteer with the same location and task (composite `unique`).

### 4. Create via Eloquent
```php
Assignment::create($request->validated());
```

### 5. Listing (`index`)
Return assignments with the volunteer, location, and task details:
```php
Assignment::with(['volunteer', 'workLocation', 'task'])->get();
```
Use an `AssignmentResource` that shows names, not just IDs.

### 6. The Route
```php
Route::apiResource('assignments', AssignmentController::class);
```
Inside the `auth:sanctum` group.

## ✅ Deliverables
- Successfully assign a volunteer to a work location with a task.
- Reject the assignment if any ID doesn't exist.
- Display assignments with full details (names, not IDs).

## 💡 Tips
- Use the relationships you defined in Task 03 — don't fetch data manually.
- Think: what happens if a volunteer/location/task with existing assignments is deleted? (Review `onDelete` in the migration.)

## 🔍 Acceptance Criteria
- [ ] Assignment works correctly
- [ ] `exists` validation blocks invalid data
- [ ] Listing shows names via relationships
- [ ] Protected by Sanctum
