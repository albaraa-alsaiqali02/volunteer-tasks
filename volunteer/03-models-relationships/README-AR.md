# التاسك 03 — الـ Models والعلاقات (Eloquent Relationships)

## 🎯 الهدف
إنشاء الـ Models وتعريف العلاقات بينها باستخدام Eloquent.

## 📋 المطلوب

### 1. أنشئ الـ Models
```bash
php artisan make:model WorkLocation
php artisan make:model Task
php artisan make:model Volunteer
php artisan make:model Assignment
```

### 2. عرّف `$fillable` لكل Model
حدّد الأعمدة القابلة للتعبئة (mass assignment) في كل model.

### 3. عرّف العلاقات

العلاقات المطلوبة:

| Model | العلاقة |
|-------|---------|
| `Volunteer` | `hasMany(Assignment::class)` |
| `WorkLocation` | `hasMany(Assignment::class)` |
| `Task` | `hasMany(Assignment::class)` |
| `Assignment` | `belongsTo(Volunteer)`, `belongsTo(WorkLocation)`, `belongsTo(Task)` |

> نصيحة متقدّمة: ممكن تستخدم `belongsToMany` بين `Volunteer` و `WorkLocation` عبر جدول `assignments` كـ pivot يحمل `task_id`. جرّبها وافهم الفرق.

### 4. تأكد من العلاقات في Tinker
```bash
php artisan tinker
>>> App\Models\Volunteer::with('assignments')->get();
```

## ✅ المخرجات المطلوبة
- 4 Models بعلاقات معرّفة وصحيحة.

## 💡 تلميحات
- اسم دالة العلاقة بيفرق: `hasMany` بصيغة الجمع، `belongsTo` بصيغة المفرد.
- استخدم Type Hints على دوال العلاقات لو بتقدر.

## 🔍 معايير القبول
- [ ] كل Model له `$fillable`
- [ ] كل العلاقات معرّفة
- [ ] جلبت بيانات مترابطة عبر `with()` بنجاح
