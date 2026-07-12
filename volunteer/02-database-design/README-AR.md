# التاسك 02 — تصميم قاعدة البيانات (Database Design + Migrations)

## 🎯 الهدف
تصميم بنية قاعدة البيانات للمشروع، وكتابة الـ Migrations.

## 📋 المطلوب

### 1. ارسم مخطط ERD
حدّد الجداول والعلاقات بينها قبل ما تكتب أي كود. ارفع صورة المخطط بالمجلد.

### 2. الجداول المطلوبة (الحد الأدنى)

| الجدول | أهم الأعمدة |
|--------|-------------|
| `users` | id, name, email, password (موجود أصلًا — للـ Admin) |
| `work_locations` | id, name, address (اختياري), timestamps |
| `tasks` | id, name, description (اختياري), timestamps |
| `volunteers` | id, name, phone, email (اختياري), timestamps |
| `assignments` | id, volunteer_id, work_location_id, task_id, notes (اختياري), timestamps |

> جدول `assignments` هو اللي بيربط: **متطوع + مكان عمل + مهمة**.

### 3. اكتب الـ Migrations
- كل جدول بـ migration خاص فيه.
- استخدم **Foreign Keys** في `assignments`:
  - `volunteer_id` → references `volunteers`
  - `work_location_id` → references `work_locations`
  - `task_id` → references `tasks`
- فكّر في سلوك الحذف (`onDelete('cascade')` أو `restrict`) واكتب سبب اختيارك بالـ commit.

### 4. شغّل
```bash
php artisan migrate
```

## ✅ المخرجات المطلوبة
- صورة ERD داخل المجلد.
- Migrations كاملة وشغّالة.

## 💡 تلميحات
- خلّي أسماء الجداول **جمع** وأسماء الأعمدة snake_case.
- لا تنسى `unsignedBigInteger` / `foreignId()->constrained()` للمفاتيح الأجنبية.

## 🔍 معايير القبول
- [ ] ERD واضح ومنطقي
- [ ] كل الجداول موجودة
- [ ] Foreign keys معرّفة صح
- [ ] `migrate` نجح بدون أخطاء
