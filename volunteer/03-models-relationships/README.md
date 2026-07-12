# Task 03 — Models & Eloquent Relationships

## 🎯 Goal
Create the Models and define the relationships between them using Eloquent.

## 📋 Requirements

### 1. Create the Models
```bash
php artisan make:model WorkLocation
php artisan make:model Task
php artisan make:model Volunteer
php artisan make:model Assignment
```

### 2. Define `$fillable` for each Model
Specify the mass-assignable columns in each model.

### 3. Define the relationships

| Model | Relationship |
|-------|--------------|
| `Volunteer` | `hasMany(Assignment::class)` |
| `WorkLocation` | `hasMany(Assignment::class)` |
| `Task` | `hasMany(Assignment::class)` |
| `Assignment` | `belongsTo(Volunteer)`, `belongsTo(WorkLocation)`, `belongsTo(Task)` |

> Advanced tip: you can use `belongsToMany` between `Volunteer` and `WorkLocation` through the `assignments` pivot carrying `task_id`. Try it and understand the difference.

### 4. Verify the relationships in Tinker
```bash
php artisan tinker
>>> App\Models\Volunteer::with('assignments')->get();
```

## ✅ Deliverables
- 4 Models with defined, correct relationships.

## 💡 Tips
- Relationship method names matter: `hasMany` is plural, `belongsTo` is singular.
- Use return type hints on relationship methods if you can.

## 🔍 Acceptance Criteria
- [ ] Every Model has `$fillable`
- [ ] All relationships are defined
- [ ] You fetched related data via `with()` successfully
