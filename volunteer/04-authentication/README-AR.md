# التاسك 04 — المصادقة (Login / Logout عبر Sanctum)

## 🎯 الهدف
بناء تسجيل دخول وخروج للـ Admin باستخدام Laravel Sanctum (Token-based).

## 📋 المطلوب

### 1. ثبّت وفعّل Sanctum
```bash
php artisan install:api
```
(أو حسب نسخة Laravel: `composer require laravel/sanctum` ثم نشر الإعدادات).

### 2. أضف الـ Admin يدويًا
المنصة يديرها مستخدم واحد، **يُضاف يدويًا** عبر Seeder:
```bash
php artisan make:seeder AdminSeeder
```
- داخل الـ Seeder أنشئ مستخدم واحد بإيميل وباسوورد معروفين.
- استخدم `Hash::make()` للباسوورد.
- شغّل: `php artisan db:seed --class=AdminSeeder`

### 3. أنشئ AuthController
```bash
php artisan make:controller AuthController
```
عرّف:
- `login()` — يستقبل email + password، يتحقق منهم، ويرجّع **token**.
- `logout()` — يحذف التوكن الحالي.

### 4. الـ Routes
في `routes/api.php`:
- `POST /api/login` — عام.
- `POST /api/logout` — محمي بـ `auth:sanctum`.

### 5. احمِ باقي الـ routes
كل routes الإدارة (أماكن العمل، المهام، المتطوعين، التنسيب) لازم تكون داخل:
```php
Route::middleware('auth:sanctum')->group(function () {
    // ...
});
```

## ✅ المخرجات المطلوبة
- Login يرجّع token صالح.
- Logout يلغي التوكن.
- الـ routes المحمية بترفض الطلب بدون token.

## 💡 تلميحات
- جرّب الطلبات على Postman وخزّن التوكن واستخدمه بالـ Header:
  `Authorization: Bearer {token}`
- لا تخزّن كلمات السر plain text أبدًا.

## 🔍 معايير القبول
- [ ] Admin مُضاف عبر Seeder
- [ ] Login شغّال ويرجّع token
- [ ] Logout يلغي التوكن
- [ ] Routes المحمية ما بتفتح بدون token
