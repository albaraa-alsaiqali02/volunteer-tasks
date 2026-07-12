# التاسك 09 — التشطيب والمعايير (Finalize & Standards)

## 🎯 الهدف
مراجعة المشروع كامل والتأكد إنه مرتّب، منظّم، وبمعايير احترافية قبل التسليم النهائي.

## 📋 المطلوب

### 1. توحيد شكل الردود (JSON)
- كل الـ endpoints ترجّع JSON بنفس الشكل (data / message / status).
- تأكد إن كل response عبر **API Resource**.

### 2. معالجة الأخطاء
- 404 لما العنصر غير موجود.
- 422 لأخطاء الـ Validation (Laravel بيعملها تلقائيًا مع Form Requests).
- 401 للطلبات غير المصادَق عليها.

### 3. تنظيف الكود
- لا يوجد منطق ثقيل داخل الـ Controllers (افصله لـ Service لو لزم).
- لا يوجد كود مكرّر.
- لا يوجد `dd()` أو `dump()` أو كود تجريبي.

### 4. تنظيم الملفات (راجع)
```
app/
 ├─ Http/
 │   ├─ Controllers/   (AuthController, WorkLocationController, ...)
 │   ├─ Requests/      (StoreXxxRequest, UpdateXxxRequest)
 │   └─ Resources/     (XxxResource)
 ├─ Models/            (User, WorkLocation, Task, Volunteer, Assignment)
database/
 ├─ migrations/
 └─ seeders/           (AdminSeeder)
routes/
 └─ api.php
```

### 5. التوثيق
- أضف ملف `README.md` للمشروع نفسه فيه:
  - طريقة التشغيل (install, migrate, seed, serve).
  - قائمة الـ endpoints.
  - بيانات دخول الـ Admin (للتجربة).
- (ممتاز) صدّر مجموعة Postman وارفعها.

### 6. (اختياري / Bonus) الفرونت إند
- اربط قالب فرونت جاهز بسيط مع الـ API.

## ✅ المخرجات المطلوبة
- مشروع نظيف، منظّم، موثّق، وكل الـ endpoints شغّالة.

## 🔍 الـ Checklist النهائي
- [ ] هيكلة الملفات منظّمة
- [ ] Eloquent + Relationships مستخدمة في كل مكان
- [ ] Validation كاملة (Form Requests)
- [ ] Resources لكل الردود
- [ ] المصادقة تحمي كل routes الإدارة
- [ ] منطق التنسيب صحيح
- [ ] README للمشروع + Postman collection
- [ ] Git history نظيف
