# Task 01 — Project Setup

## 🎯 Goal
Set up your working environment and an empty, ready-to-build Laravel project, connected to Git.

## 📋 Requirements
1. Create a new Laravel project:
   ```bash
   composer create-project laravel/laravel volunteer-coordination
   ```
2. Configure your `.env` file:
   - App name (`APP_NAME`).
   - Database settings (`DB_*`) — create an empty database with a clear name.
3. Run the project and make sure it opens:
   ```bash
   php artisan serve
   ```
4. Make sure `php artisan migrate` runs without errors (Laravel's default tables).
5. Connect the project to Git:
   - `git init`
   - Make sure `.gitignore` includes `.env`, `vendor/`, and `node_modules/`.
   - First commit: `task-01: project setup`.

## ✅ Deliverables
- A working Laravel project.
- A successful database connection.
- A Git repo with a clean first commit.

## 💡 Tips
- Never commit `.env` — use `.env.example` instead.
- Use PHP 8.2 or newer.

## 🔍 Acceptance Criteria
- [ ] The project opens in the browser
- [ ] `migrate` succeeded
- [ ] `.gitignore` is set and `.env` is not committed
