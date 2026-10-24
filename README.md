# ARSTT — zero-byte entrypoint repair

[Download the small repair ZIP](https://github.com/mohamed15ahmed/ARSTT/raw/refs/heads/delivery/arstt-startup-fix/ARSTT-Startup-Fix.zip)

الإصلاح لحالة app Restarting(0) بلا logs مع dist/server/main.js صفر بايت. فك التصحيح وانسخ محتوياته داخل **مجلد المشروع الموجود الذي يحتوي compose.yaml**، ووافق على الاستبدال. أغلق تشغيل المصدر المؤقت بـCtrl+C، ثم شغّل REPAIR-WINDOWS.cmd مع Docker Desktop مفتوحًا.

لا تحتاج إعادة تنزيل المشروع أو الأدوات أو إعادة تدريب النموذج. التصحيح يبني ملفات التشغيل من الصورة المحلية بلا شبكة، ويتحقق من محتواها وصياغتها، ثم يعيد إنشاء app وينتظر healthy. لا يحذف named volume أو نتائج الفحوصات ولا يغير خدمات التدريب أو أدوات Recon أو قاعدة البيانات.

ثبت أن main.js كان فارغًا على جهاز المستخدم؛ سبب كتابة صفر بايت على Docker Desktop لم يحسم. أُبقي TypeScript للتحقق من الأنواع، وأضيف مسار تحويل يعتمد esbuild0.28.2 الموجود أصلًا وكتابة Node atomic ثم التحقق من artifacts. لم تتغير إصدارات المكتبات المثبتة.

اختُبرت حالة الملف الفارغ وإصلاحه، وبناء Docker offline، وبدء HTTP والواجهة وتحميل الموديل وفحصا Headers/Nmap الحقيقيان على المختبر. Windows/CMD نفسه لم يُختبر في هذه البيئة Linux. [التقرير](STARTUP-FIX.ar.md)، والأدلة في Repair-Test-Evidence.zip.
