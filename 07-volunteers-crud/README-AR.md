# التاسك 07 — المتطوعين CRUD (Volunteers)

## 🎯 الهدف
بناء CRUD كامل للمتطوعين.

## 📋 المطلوب
نفس النمط من [التاسك 05](../05-work-locations-crud):

1. `VolunteerController` (`--api`).
2. Form Requests: `StoreVolunteerRequest`, `UpdateVolunteerRequest`.
3. `VolunteerResource`.
4. Route: `Route::apiResource('volunteers', VolunteerController::class);` داخل مجموعة `auth:sanctum`.

### الـ Endpoints
| Method | Endpoint |
|--------|----------|
| GET | `/api/volunteers` |
| POST | `/api/volunteers` |
| GET | `/api/volunteers/{id}` |
| PUT/PATCH | `/api/volunteers/{id}` |
| DELETE | `/api/volunteers/{id}` |

## ✅ المخرجات المطلوبة
- CRUD كامل للمتطوعين شغّال ومحمي.

## 💡 تلميحات
- في `index` فكّر تعرض مع كل متطوع تنسيباته (`with('assignments')`) — مفيد للتاسك الجاي.
- تحقق من صيغة الإيميل/الهاتف في الـ Validation.

## 🔍 معايير القبول
- [ ] CRUD شغّال
- [ ] Validation + Resource مستخدمين
- [ ] محمي بـ Sanctum
