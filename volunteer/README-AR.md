# مشروع تنسيق المتطوعين — Volunteer Coordination (Laravel Back-End)

مشروع تدريبي لمسار **الباك إند (Laravel)** ضمن البرنامج التدريبي للطلاب.
الهدف بناء **تطبيق ويب صغير (API)** لتنسيق المتطوعين وتوزيعهم على المهام في أماكن العمل.

> اقرأ ملف [`PROJECT-AR.md`](./PROJECT-AR.md) أول إشي — فيه وصف المشروع الكامل ومتطلباته.

---

## فكرة المشروع باختصار

منصّة يديرها **مستخدم واحد (Admin)** يتم إضافته يدويًا إلى قاعدة البيانات.
الـ Admin يقدر:

- تسجيل الدخول / تسجيل الخروج.
- إدارة **أماكن العمل** (مثل: مشفى الشفاء، مركز توزيع المساعدات).
- إدارة **المهام** (مثل: التوزيع، الإدارة، الإسعاف، المراقبة).
- إدارة **المتطوعين**.
- **تنسيب متطوع** إلى مكان عمل، مع اختيار المهمة الخاصة فيه في ذاك المكان.

---

## كيف تشتغل على الريبو؟

المشروع مقسوم إلى **تاسكات متدرّجة (Tasks)**. كل تاسك يبني على اللي قبله.
ابدأ من التاسك الأول وامشي بالترتيب — كل مجلد فيه ملف `README.md` يشرح المطلوب بالتفصيل.

| # | التاسك | الموضوع |
|---|--------|---------|
| 01 | [تجهيز المشروع](./01-setup) | تنصيب Laravel + إعداد البيئة و Git |
| 02 | [تصميم قاعدة البيانات](./02-database-design) | ERD + Migrations |
| 03 | [الـ Models والعلاقات](./03-models-relationships) | Eloquent Models & Relationships |
| 04 | [المصادقة](./04-authentication) | Login / Logout (Sanctum) |
| 05 | [أماكن العمل CRUD](./05-work-locations-crud) | Work Locations endpoints |
| 06 | [المهام CRUD](./06-tasks-crud) | Tasks endpoints |
| 07 | [المتطوعين CRUD](./07-volunteers-crud) | Volunteers endpoints |
| 08 | [تنسيب المتطوعين](./08-assignment) | Assignment logic |
| 09 | [التشطيب والمعايير](./09-finalize-standards) | Validation, Resources, Standards |

ملف [`resources`](./resources) فيه روابط ومصادر تساعدك.

---

## طريقة التسليم

- كل طالب يعمل **Fork** للريبو، أو يشتغل على **branch** خاص فيه باسمه.
- يكون في **commit واضح** لكل تاسك (مش commit واحد بالآخر).
- رسالة الـ commit تكون واضحة، مثال: `task-04: add login & logout endpoints`.
- بعد كل تاسك، اعمل **Pull Request** ليتم مراجعته.

---

## القواعد العامة (مهمة)

1. **تقسيم الملفات** بشكل سليم ومنظّم (Controllers, Models, Requests, Resources, Routes).
2. استخدام **Eloquent** و **Eloquent Relationships** لكل عمليات قاعدة البيانات — ممنوع Query Builder الخام أو SQL مباشر إلا للضرورة.
3. **Validation** لكل مدخلات (استخدم Form Requests).
4. ردود الـ API تكون **JSON** ثابتة الشكل (استخدم API Resources).
5. أسماء واضحة للـ routes والـ methods و الـ variables.
6. ممنوع رفع ملف `.env` أو مجلد `vendor` (تأكد من `.gitignore`).

---

## المتطلبات التقنية

- PHP 8.2+
- Laravel 11/12
- قاعدة بيانات: MySQL أو PostgreSQL
- Laravel Sanctum (للمصادقة عبر التوكن)
- أداة لاختبار الـ API: Postman / Insomnia

---

## معايير التقييم (Checklist عامة)

- [ ] هيكلة المشروع منظّمة ومعايير واضحة
- [ ] العلاقات (Relationships) معرّفة وصحيحة
- [ ] كل الـ CRUD endpoints شغّالة
- [ ] المصادقة شغّالة وتحمي الـ routes
- [ ] منطق التنسيب صحيح (متطوع + مكان عمل + مهمة)
- [ ] Validation موجودة وكافية
- [ ] الردود JSON منظّمة (Resources)
- [ ] Git history نظيف وكل تاسك بـ commit واضح
