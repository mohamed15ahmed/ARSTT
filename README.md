# ARSTT 1.1 — Complete source delivery

[Download the complete project ZIP](https://github.com/mohamed15ahmed/ARSTT/raw/refs/heads/delivery/arstt-complete-1.1/ARSTT-Complete-1.1.zip)

فك ZIP بالكامل، شغّل Docker Desktop مع WSL2 وLinux containers، ثم START-WINDOWS.cmd. افتح http://localhost:4310. التشغيل الأول ينزل الأدوات الرسمية ويتحقق من بصماتها؛ خصص8GB RAM و15GB مساحة على الأقل. يوجد تدريب معزول http://juice-shop:3000/ من داخل التطبيق وhttp://localhost:3000 من Windows.

تشمل النسخة Recon المنقول، Nmap Profiles، JS/WhatWeb/TLS/Nuclei/ZAP، HTTP checks، DAST محلي محدود، Jobs وTerminal حقيقيين، SQLite، النتائج والمقارنة والتقارير JSON/CSV/PDF، ونموذج ML محلي فعلي دون API Key. النموذج TF-IDF + Logistic Regression؛ train23869/validation5142/test5074؛ accuracy87.27% وmacroF1 .8513. التنبؤ تصنيف نصي مساعد، وليس إثبات ثغرة أو توقع exploitability. المصدر GitHub Advisory Database CC-BY-4.0 وإعادة التدريب موثقة داخل المشروع.

19 اختبار Backend وأدوات و25 فحص واجهة نجحوا، بالإضافة لتشغيل الأدوات على Juice Shop وقراءة التقارير وحفظ77 سجلًا بعد إعادة التشغيل. Windows نفسه والنطاقات العامة لم يُختبرا. Subfinder/Wayback يتخطيان أسماء المختبر الداخلية بشكل صحيح. لا تفحص أهدافًا غير مصرح بها. التفاصيل في [التقرير](VERIFICATION.ar.md) و[ملف النموذج](MODEL.ar.md)، والأدلة الأصلية في ARSTT-1.1-Test-Evidence.zip.

هذا فرع تسليم مستقل، دون استبدال main أو حذف المشاريع والسجلات السابقة. لا تشمل الحزمة node_modules أو binaries مثبتة أو بيانات المستخدم؛ البناء داخل Docker يجهزها. لا تستخدم docker compose down -v لأنه يحذف بيانات الفحوصات.
