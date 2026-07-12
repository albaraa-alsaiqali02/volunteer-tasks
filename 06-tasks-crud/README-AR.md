# التاسك 06 — المهام CRUD (Tasks)

## 🎯 الهدف
بناء CRUD كامل للمهام (مثل: التوزيع، الإدارة، الإسعاف، المراقبة).

## 📋 المطلوب
كرّر نفس النمط من [التاسك 05](../05-work-locations-crud):

1. `TaskController` (`--api`).
2. Form Requests: `StoreTaskRequest`, `UpdateTaskRequest`.
3. `TaskResource`.
4. Route: `Route::apiResource('tasks', TaskController::class);` داخل مجموعة `auth:sanctum`.

### الـ Endpoints
| Method | Endpoint |
|--------|----------|
| GET | `/api/tasks` |
| POST | `/api/tasks` |
| GET | `/api/tasks/{id}` |
| PUT/PATCH | `/api/tasks/{id}` |
| DELETE | `/api/tasks/{id}` |

## ✅ المخرجات المطلوبة
- CRUD كامل للمهام شغّال ومحمي.

## 💡 تلميحات
- لا تكرّر الكود حرفيًا — افهم النمط وطبّقه.
- خلّي `name` مطلوب وفريد (`unique`) لو منطقي إن أسماء المهام ما تتكرر.

## 🔍 معايير القبول
- [ ] CRUD شغّال
- [ ] Validation + Resource مستخدمين
- [ ] محمي بـ Sanctum
