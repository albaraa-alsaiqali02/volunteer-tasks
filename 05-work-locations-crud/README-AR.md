# التاسك 05 — أماكن العمل CRUD (Work Locations)

> هاد التاسك هو **النموذج (Pattern)** اللي رح تكرّره في تاسك المهام والمتطوعين. ركّز فيه منيح.

## 🎯 الهدف
بناء CRUD كامل لأماكن العمل (إضافة / عرض / تعديل / حذف).

## 📋 المطلوب

### 1. الـ Controller
```bash
php artisan make:controller WorkLocationController --api
```
عرّف الدوال: `index`, `store`, `show`, `update`, `destroy`.

### 2. الـ Validation (Form Request)
```bash
php artisan make:request StoreWorkLocationRequest
php artisan make:request UpdateWorkLocationRequest
```
- ضع قواعد التحقق (مثلًا `name` required|string|max:255).
- استخدمها داخل الـ Controller بدل ما تكتب validation داخل الدالة.

### 3. الـ API Resource
```bash
php artisan make:resource WorkLocationResource
```
- حدّد شكل الـ JSON اللي بترجع للـ client (لا ترجّع الـ model خام).

### 4. الـ Routes
في `routes/api.php` داخل مجموعة `auth:sanctum`:
```php
Route::apiResource('work-locations', WorkLocationController::class);
```

### 5. الـ Endpoints الناتجة
| Method | Endpoint | الوظيفة |
|--------|----------|---------|
| GET | `/api/work-locations` | عرض الكل |
| POST | `/api/work-locations` | إضافة |
| GET | `/api/work-locations/{id}` | عرض واحد |
| PUT/PATCH | `/api/work-locations/{id}` | تعديل |
| DELETE | `/api/work-locations/{id}` | حذف |

## ✅ المخرجات المطلوبة
- 5 endpoints شغّالة ومحمية بالمصادقة.
- Validation تشتغل وترجّع أخطاء واضحة.
- الردود عبر Resource.

## 💡 تلميحات
- استخدم Eloquent: `WorkLocation::create()`, `->update()`, `->delete()`.
- استخدم **Route Model Binding** (مرّر `WorkLocation $workLocation` بدل `$id`).
- رجّع status codes صحيحة (201 للإنشاء، 404 لغير الموجود…).

## 🔍 معايير القبول
- [ ] كل الـ CRUD شغّال
- [ ] Form Requests مستخدمة
- [ ] Resource مستخدم
- [ ] محمي بـ Sanctum
