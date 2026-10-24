# تقرير تحقق ARSTT 1.1 — 24 أكتوبر 2026

هذه المراجعة تشمل المشروع الجديد وتكامل Recon المنقول من HunterKit. التقارير في revision تاريخية ولا تصف حدود النسخة الحالية. الأدلة المرفقة مستقلة عن بيانات المستخدم وتنتج من أدوات حقيقية على fixtures محلية وOWASP Juice Shop v19 معزول.

## التغطية والنتيجة

| الاختبار | الحالة | الدليل/المصدر |
|---|---|---|
| جاهزية أدوات صورة التسليم | Passed | release-doctor.log؛ كل executable بدأ فعليًا، فحص ZAP مشترك للوضعيْن |
| TypeScript + Vite + Backend build | Passed | final-build.log، بلا تغيير إصدارات لمجرد التحديث |
| اختبارات Backend/API/SQLite/Processes/ML/Scope | Passed | 13 tests، final-unit.log وfinal-docker-unit.log |
| Recon المنقول: HTTPX/inventory/screenshot وعزل الأهداف | Passed | final-docker-recon.log، recon-port-acceptance.json |
| أدوات حقيقية: Nmap/JS/WhatWeb/TLS/Nuclei/ZAP | Passed | final-docker-tools.log: 2 tests، TLS مرتان، فشل/مهلة وعزل نتائج لكل أداة |
| Nmap quick/basic/detailed على منفذين مصرح بهما | Passed | network-profiles.json؛ المنافذ والخدمات من Nmap نفسه |
| Headers/Cookies/CORS/API على Juice Shop | Passed | complete-deployment.json؛ مخرجات HTTP فعلية |
| ZAP Active SQLi/XSS محدود على Juice Shop المعزول | Passed | complete-deployment.json؛ انتهاء activeScan وpassive wait وخروج ناجح وتقرير صالح |
| وصول حساب B إلى سلة A داخل Juice Shop | Passed | دليل crossUserReadReturnedFirstUserObject، بلا حفظ credentials |
| فشل/مهلة/إلغاء DAST | Passed | dast-timeout-cancel.json؛ لا نجاح وهمي |
| JSON/CSV/PDF وتصدير فحص فاشل | Passed | exports وpdf-acceptance.json؛ PDF قُرئ بمحلل مستقل ورُسم وفُتح بصريًا |
| الواجهة والتنقل/السجل/SSE/الشاشات | Passed | browser-acceptance.json: 14 checks، full-browser-acceptance.json: 11 checks |
| npm run dev: UI+API+proxy+CSRF | Passed | dev-acceptance.json؛ المنافذ5175/4311 |
| تدريب ML وتقييم Test مستقل وتحميله داخل Docker | Passed | evaluation-check.json، accuracy=.872684، macroF1=.851285، overlap=0 |
| فشل ML لا يعطل الفحص أو يبدل أدلته | Passed | complete.test.ts، analysis محفوظة منفصلة |
| CVE references | Passed | references.test.ts؛ NVD أو local reviewed advisory fallback، تطابق الإصدار ليس إثبات استغلال |
| npm production audit | Passed | final-audit.json: صفر advisories وقت التنفيذ |
| بقاء البيانات بعد restart | Passed | restart-persistence-full.json؛ 77 Jobs و8 مشاريع مع analysis/review states بلا تغيير |
| Windows/Docker Desktop/WSL2 فعليًا | Not Tested | المضيف المتاح Linux؛ المسار موثق وDocker amd64 مختبر |
| تعداد Subfinder/Wayback لنطاق عام | Not Tested | لا نطاق عام مصرح به متاح؛ CLI مثبتة ومختبرة، والمحلي Skipped بشكل صحيح |
| كامل قوالب Nuclei أو كل قواعد ZAP | Not Tested | profiles محدودة ومعلنة وليست فحصًا شاملًا لكل أنواع الثغرات |

نجاح اختبار الأداة على مختبر لا يضمن نجاح اتصالها بكل هدف ولا يثبت خلو المشروع أو الأهداف من كل خطأ. حالات المهلة والفشل تظهر بأسبابها وتحفظ سجلها.

## الملفات الرئيسية التي تغيرت وسببها

- server/types.ts/store.ts/app.ts: Profiles ومنافذ مصرح بها، ترحيل SQLite إضافي مع backup، حالات مراجعة، تحليل مستقل، API للمقارنة والتقارير والموديل.
- server/runner.ts/parsers.ts: Profiles حقيقية وCPE، تنفيذ ZAP النشط المحدود والتحقق من اكتمال المخرجات.
- server/tools.ts: اكتشاف الفحوص المضافة وإعادة استخدام فحص إصدار ZAP للوضعيْن بدل تشغيل Java مرتين لنفس executable.
- server/checks.ts: فحوص HTTP ودليل وصول بحسابين، محصورة في المختبر للوظائف المؤثرة.
- server/analysis.ts: تجميع العرض وعزل الهدف والمقارنة وحساب Review Index ومراجع CVE.
- server/ml.ts وml/: تدريب وإعادة إنتاج وتقييم وتحميل واستدلال فعلي دون API Key.
- server/pdf.ts: PDF محلي sandbox، تعطيل JavaScript والشبكة وتنقيح البيانات.
- models/: weights حقيقية وmetrics وsplit manifests وlicense وconfusion matrix وindex مرخص.
- client/main.tsx وInsights.tsx وstyle.css: أقسام رئيسية وCards، Profiles وResults ومقارنة ونموذج وتقارير دون بيانات شكلية.
- Dockerfile/compose.yaml/START-WINDOWS.cmd: Python والموديل وChromium وبيئة Juice Shop معزولة؛ لا تثبيت أدوات الفحص على Windows مباشرة.
- tests/ وdocs/ وREADME: اختبارات قبول وتوثيق حدود الأدلة وإعادة التشغيل.

server/recon.ts الخاص بالـworkflows المنقولة لم يتغير خلال مرحلة الإكمال؛ تحفظ النسخة القديمة ومجلدها الأصلي. الملفات المشتركة أضيف لها دعم الأدوات الجديدة؛ اختبار Recon الحقيقي أعيد بعد الإضافات. تغيير مظهر الواجهة يتبع التفويض اللاحق من المستخدم لتطبيق الهوية العامة. لا تعديل لمجلد HunterKit القديم أو أوامر أدواته.

## مخاطر وحدود مهمة

التطبيق محلي لمشغل واحد، لا يستبدل منصة SaaS ذات مصادقة متعددة المستخدمين. صلاحية الفحص إقرار مشغل ونطاق origin+منافذ معتمدة. فحوص التأثير الفعلي مقفلة على Juice Shop داخل Docker، لا هدف الجامعة أو demo عام. خدمة التدريب تتعمد وجود ثغرات، وتبقى على شبكة داخلية ومنافذ loopback فقط.

النموذج يتعلم تصنيف نوع النص من advisories إنجليزية، لا تأكيد ثغرة أو severity حقيقية. Text distribution قد تختلف عن نتائج scanners؛ الامتناع عند قلة النص أو الاحتمال المتدني سلوك مقصود. 87.27% تخص Test لهذه المهمة فقط. تجربة الشدة الأضعف محفوظة ومعلنة وغير مستخدمة لتعديل الأصل.

سعة القرص تحد التخزين؛ لا حذف تلقائي للسجل. اسرار الأدوات تُنقح بأنماط معروفة، ولا يوجد ضمان كشف كل سر محتمل. القوالب والبناء الأول تتطلب الشبكة؛ التشغيل بالموديل المحلي لا يحتاج مفتاح AI. استرجاع البيئة السحابية من snapshot جديدة لم يُختبر، وهو مستقل عن ZIP المشروع.

## صورة التسليم وربط الاختبارات بالمصدر

صورة Docker النهائية: `sha256:a64e3711b3e5ad4341aa11ffc3beb93f1d2fd3014cc5b09736d12567e2745a4b`. أُعيد فحص الجاهزية وNmap profiles وPDF والفشل/المهلة/الإلغاء والمتصفح بعد الإصلاح النهائي. اختبار الأدوات الطويل واختبار Juice Shop الشامل سبقا تحسين عرض PDF وتجميع نتائج الواجهة وإزالة تكرار فحص إصدار ZAP؛ ملفات التنفيذ والمحللات والموديل نفسها تطابقت بالبصمات مع صورة التسليم. أدلة المطابقة في integration-source-sha.json وrelease-image-source-integrity.json.

تعثر بناء وسيط بسبب نفاد مساحة Docker VFS في السحابة. نُظفت صور هذا العمل المؤقتة فقط ثم نجح البناء النهائي؛ لم تُحذف قاعدة البيانات أو named volume. هذه ليست مشكلة أداة أخفيت بحالة نجاح.

## فحوص Juice Shop الفعلية

الأعداد observations وليست عدد ثغرات مؤكدة. لكل عملية أصل مستقل ومعرف Job في complete-deployment.json.

| الأداة | الحالة الفعلية | observations |
|---|---|---|
| recon | completed | 8 |
| subfinder | skipped | 0 |
| httpx | completed | 1 |
| screenshots | completed | 1 |
| wayback | skipped | 0 |
| recon-js | completed | 4 |
| parameters | completed | 0 |
| jsleaks | completed | 43 |
| nmap | completed | 1 |
| whatweb | completed | 8 |
| nuclei | completed | 8 |
| zap | completed | 6 |
| headers | completed | 4 |
| cookies | completed | 1 |
| cors | completed | 1 |
| api-check | completed | 1 |
| access-control | completed | 1 |
| zap-active | completed | 7 |
| headers | completed | 4 |

TLS اختُبر مرتين على شهادة fixture محلية موقعة ذاتيًا؛ لم يُجرَ TLS على HTTP Juice Shop. Subfinder وWayback ليسا فحوصًا للنطاقات الداخلية؛ Skipped حالة صحيحة وغير مساوية لنجاح تعداد عام.
