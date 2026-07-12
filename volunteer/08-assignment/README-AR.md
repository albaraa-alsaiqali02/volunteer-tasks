# التاسك 08 — تنسيب المتطوعين (Assignment)

> هاد قلب المشروع. التاسكات اللي قبل كانت تجهيز لهاد.

## 🎯 الهدف
تنسيب متطوع إلى **مكان عمل** مُضاف مسبقًا، مع اختيار **مهمته** (من المهام المُضافة مسبقًا) في ذلك المكان.

## 📋 المطلوب

### 1. الـ Controller
```bash
php artisan make:controller AssignmentController --api
```
على الأقل: `index`, `store`, `destroy` (وممكن `update` لتغيير المهمة/المكان).

### 2. منطق التنسيب (`store`)
الـ request لازم يحتوي:
- `volunteer_id`
- `work_location_id`
- `task_id`

### 3. الـ Validation (مهمة جدًا)
في `StoreAssignmentRequest`:
- الثلاث IDs **موجودين فعلًا** بقواعد البيانات → استخدم `exists`:
  ```php
  'volunteer_id'      => 'required|exists:volunteers,id',
  'work_location_id'  => 'required|exists:work_locations,id',
  'task_id'           => 'required|exists:tasks,id',
  ```
- (اختياري متقدّم) امنع تكرار نفس التنسيب لنفس المتطوع بنفس المكان والمهمة (`unique` مركّب).

### 4. الإنشاء عبر Eloquent
```php
Assignment::create($request->validated());
```

### 5. العرض (`index`)
ارجِع التنسيبات مع تفاصيل المتطوع والمكان والمهمة:
```php
Assignment::with(['volunteer', 'workLocation', 'task'])->get();
```
استخدم `AssignmentResource` يعرض الأسماء مش بس الـ IDs.

### 6. الـ Route
```php
Route::apiResource('assignments', AssignmentController::class);
```
داخل مجموعة `auth:sanctum`.

## ✅ المخرجات المطلوبة
- تنسيب متطوع لمكان عمل مع مهمة، بنجاح.
- رفض التنسيب لو أي ID غير موجود.
- عرض التنسيبات بتفاصيلها الكاملة (أسماء، مش IDs).

## 💡 تلميحات
- استغل العلاقات اللي عرّفتها في التاسك 03 — لا تجلب البيانات يدويًا.
- فكّر: شو يصير لو انحذف متطوع/مكان/مهمة عندها تنسيبات؟ (راجع `onDelete` بالـ migration).

## 🔍 معايير القبول
- [ ] التنسيب يشتغل صح
- [ ] Validation بـ `exists` تمنع البيانات الغلط
- [ ] العرض يطلّع الأسماء عبر العلاقات
- [ ] محمي بـ Sanctum
