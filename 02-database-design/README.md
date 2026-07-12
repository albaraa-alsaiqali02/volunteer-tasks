# Task 02 — Database Design (Migrations)

## 🎯 Goal
Design the project's database structure and write the Migrations.

## 📋 Requirements

### 1. Draw an ERD
Decide on the tables and relationships **before** writing any code. Add the diagram image to this folder.

### 2. Required tables (minimum)

| Table | Key columns |
|-------|-------------|
| `users` | id, name, email, password (already exists — for the Admin) |
| `work_locations` | id, name, address (optional), timestamps |
| `tasks` | id, name, description (optional), timestamps |
| `volunteers` | id, name, phone, email (optional), timestamps |
| `assignments` | id, volunteer_id, work_location_id, task_id, notes (optional), timestamps |

> The `assignments` table is what links: **volunteer + work location + task**.

### 3. Write the Migrations
- One migration per table.
- Use **foreign keys** in `assignments`:
  - `volunteer_id` → references `volunteers`
  - `work_location_id` → references `work_locations`
  - `task_id` → references `tasks`
- Decide on the delete behavior (`onDelete('cascade')` vs `restrict`) and explain your choice in the commit.

### 4. Run
```bash
php artisan migrate
```

## ✅ Deliverables
- An ERD image inside this folder.
- Complete, working migrations.

## 💡 Tips
- Use **plural** table names and snake_case columns.
- Don't forget `foreignId()->constrained()` for foreign keys.

## 🔍 Acceptance Criteria
- [ ] Clear, logical ERD
- [ ] All tables exist
- [ ] Foreign keys defined correctly
- [ ] `migrate` runs with no errors
