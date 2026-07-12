# Task 04 — Authentication (Login / Logout with Sanctum)

## 🎯 Goal
Build login and logout for the Admin using Laravel Sanctum (token-based).

## 📋 Requirements

### 1. Install and enable Sanctum
```bash
php artisan install:api
```
(Or depending on your Laravel version: `composer require laravel/sanctum`, then publish the config.)

### 2. Add the Admin manually
The platform is managed by a single user, **added manually** via a Seeder:
```bash
php artisan make:seeder AdminSeeder
```
- Inside the Seeder, create one user with a known email and password.
- Use `Hash::make()` for the password.
- Run: `php artisan db:seed --class=AdminSeeder`

### 3. Create AuthController
```bash
php artisan make:controller AuthController
```
Define:
- `login()` — receives email + password, validates them, returns a **token**.
- `logout()` — deletes the current token.

### 4. Routes
In `routes/api.php`:
- `POST /api/login` — public.
- `POST /api/logout` — protected by `auth:sanctum`.

### 5. Protect the rest of the routes
All admin routes (work locations, tasks, volunteers, assignment) must be inside:
```php
Route::middleware('auth:sanctum')->group(function () {
    // ...
});
```

## ✅ Deliverables
- Login returns a valid token.
- Logout revokes the token.
- Protected routes reject requests without a token.

## 💡 Tips
- Test requests in Postman, save the token, and use it in the header:
  `Authorization: Bearer {token}`
- Never store passwords in plain text.

## 🔍 Acceptance Criteria
- [ ] Admin added via Seeder
- [ ] Login works and returns a token
- [ ] Logout revokes the token
- [ ] Protected routes don't open without a token
