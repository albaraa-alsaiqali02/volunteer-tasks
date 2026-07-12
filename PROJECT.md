# Full Project Description

## Project Nature

Develop a small **web application** using **Laravel**.

### Frontend

Preferably use a **ready-made frontend template** (optional — the main focus is the back-end / API).

---

## Project Requirements

The project is a small web application **for coordinating volunteers**, distributing them across tasks at work locations.

The platform is managed by a **single user (Admin)**, who is added **manually** to the database (via a Seeder).

Based on that, the required **API requests** are:

1. **Login** and **logout**.
2. **Work locations** — add / edit / delete / view
   (e.g., Al-Shifa Hospital, aid distribution center, etc.).
3. **Tasks** — add / edit / delete / view
   (e.g., distribution, management, first aid, monitoring).
4. **Volunteers** — add / edit / delete / view.
5. **Assign a volunteer** to a previously added work location;
   choosing their **task** (from the previously added tasks) at that location.

---

## Important Notes

- Make sure to **organize the project files** properly with clear standards.
- Use **Eloquent** and **Eloquent Relationships** for database operations.

---

## Expected Entities

| Entity | Description |
|--------|-------------|
| `User` | The single Admin who manages the platform |
| `WorkLocation` | A work location (hospital, distribution center…) |
| `Task` | A task (distribution, first aid, monitoring…) |
| `Volunteer` | A volunteer |
| `Assignment` | Links a volunteer + work location + task |

> This breakdown is a suggested and logical model for the project — we will detail it in the database design task.
