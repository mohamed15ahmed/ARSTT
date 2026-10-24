# ARSTT — تحديث مرحلة Recon

[تحميل النسخة الجديدة ZIP](https://github.com/mohamed15ahmed/ARSTT/raw/refs/heads/delivery/arstt-recon-update/ARSTT-recon-update.zip)

فك ZIP بالكامل، وافتح مجلد ARSTT، وشغّل Docker Desktop بوضع Linux/WSL2، ثم START-WINDOWS.cmd. أول بناء يحتاج إنترنت. إذا كنت تستخدم نسخة سابقة، أوقف حاوياتها أولًا لتجنب تعارض المنافذ، دون حذف volumes أو نتائجك.

Recon المنقولة من HunterKit، Cards، Results، إصلاحات redirects وPartial/Skipped، وJuice Shop المحلي مضمنة. راجع [تقرير المرحلة والاختبارات](RECON-RESULT.ar.md). هذه مرحلة Recon وليست اكتمال جميع متطلبات المشروع: active DAST وAI المدرّب وPDF والمقارنة ما زالت غير مكتملة.

هدف التدريب داخل Docker: http://juice-shop:3000/، والـfixture الآخر http://lab:8088/. لا تستخدم نسخة demo العامة للاختبارات المؤثرة. Windows مباشرة لم يُختبر؛ الاختبارات الفعلية Docker/Linux.

الفرع منفصل ولا يستبدل main. البصمة المرفقة للتحقق من سلامة ZIP. لا تتضمن الحزمة node_modules أو binaries مثبتة أو بيانات مستخدم؛ Docker يبني بيئة الأدوات من الملفات الرسمية المثبتة البصمات.
