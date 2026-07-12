# التاسك 01 — تجهيز المشروع (Setup)

## 🎯 الهدف
تجهيز بيئة العمل ومشروع Laravel فاضي وجاهز للشغل، وربطه بـ Git.

## 📋 المطلوب
1. أنشئ مشروع Laravel جديد:
   ```bash
   composer create-project laravel/laravel volunteer-coordination
   ```
2. اضبط ملف `.env`:
   - اسم التطبيق `APP_NAME`.
   - إعدادات قاعدة البيانات (`DB_*`) — أنشئ قاعدة بيانات فاضية باسم واضح.
3. شغّل المشروع وتأكد إنه يفتح:
   ```bash
   php artisan serve
   ```
4. تأكد إن `php artisan migrate` يشتغل بدون أخطاء (جداول Laravel الافتراضية).
5. اربط المشروع بـ Git:
   - `git init`
   - تأكد إن `.gitignore` فيه `.env` و `vendor/` و `node_modules/`.
   - أول commit: `task-01: project setup`.

## ✅ المخرجات المطلوبة
- مشروع Laravel شغّال.
- اتصال ناجح بقاعدة البيانات.
- ريبو Git مربوط وفيه commit أول نظيف.

## 💡 تلميحات
- لا ترفع `.env` أبدًا — استخدم `.env.example` بدّاله.
- خلّي إصدار PHP عندك 8.2 أو أحدث.

## 🔍 معايير القبول
- [ ] المشروع يفتح على المتصفح
- [ ] `migrate` نجح
- [ ] `.gitignore` مضبوط و `.env` مش مرفوع
