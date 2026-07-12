# Task 09 — Finalize & Standards

## 🎯 Goal
Review the whole project and make sure it's tidy, organized, and professional before final submission.

## 📋 Requirements

### 1. Unify response shape (JSON)
- All endpoints return JSON in the same shape (data / message / status).
- Make sure every response goes through an **API Resource**.

### 2. Error handling
- 404 when an item is not found.
- 422 for validation errors (Laravel does this automatically with Form Requests).
- 401 for unauthenticated requests.

### 3. Clean up the code
- No heavy logic inside Controllers (extract to a Service if needed).
- No duplicated code.
- No `dd()`, `dump()`, or leftover test code.

### 4. Organize files (review)
```
app/
 ├─ Http/
 │   ├─ Controllers/   (AuthController, WorkLocationController, ...)
 │   ├─ Requests/      (StoreXxxRequest, UpdateXxxRequest)
 │   └─ Resources/     (XxxResource)
 ├─ Models/            (User, WorkLocation, Task, Volunteer, Assignment)
database/
 ├─ migrations/
 └─ seeders/           (AdminSeeder)
routes/
 └─ api.php
```

### 5. Documentation
- Add a `README.md` for the project itself containing:
  - How to run it (install, migrate, seed, serve).
  - The list of endpoints.
  - Admin login credentials (for testing).
- (Great) Export a Postman collection and include it.

### 6. (Optional / Bonus) Frontend
- Connect a simple ready-made frontend template to the API.

## ✅ Deliverables
- A clean, organized, documented project with all endpoints working.

## 🔍 Final Checklist
- [ ] File structure is organized
- [ ] Eloquent + Relationships used everywhere
- [ ] Complete validation (Form Requests)
- [ ] Resources for all responses
- [ ] Authentication protects all admin routes
- [ ] Assignment logic is correct
- [ ] Project README + Postman collection
- [ ] Clean Git history
