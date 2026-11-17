# البراشية أون لاين — دليل خدمات البلد

دليل خدمات قرية البراشية: أطباء، صيدليات، سباكة، كهرباء، مواصلات، محلات، مطاعم، وغيرها — مع بحث، وتقييمات، ومقترحات، واستفسارات.

## الروابط

- **الموقع:** https://abdullahshahino23-lgtm.github.io/barashia-online/
- **لوحة التحكم (للأدمن):** https://abdullahshahino23-lgtm.github.io/barashia-online/admin.html
- **واجهة البيانات JSON:** https://abdullahshahino23-lgtm.github.io/barashia-online/api.html?r=providers
- **صفحة فحص التطبيق:** https://abdullahshahino23-lgtm.github.io/barashia-online/check.html

## إزاي شغال

- الواجهة صفحة واحدة (`index.html`) منشورة على GitHub Pages.
- البيانات (مقدمو الخدمات، التقييمات، المقترحات، الأرقام المهمة) محفوظة في **Firebase Firestore** — مشروع `barashia-online`.
- أي حد يقدر يضيف خدمته من زر **"أضف خدمتك مجانًا"** في الموقع، ولوحة التحكم بتعرض الطلبات للموافقة عليها أو إيقافها.
- التطبيق بيحفظ نسخة من الخدمات في المتصفح (localStorage) فبيفتح فورًا ويحدّث البيانات في الخلفية.

## الملفات

| الملف | الوصف |
| --- | --- |
| `index.html` | الموقع (الرئيسية، الخدمات، الأرقام، المقترحات، الاستفسارات) |
| `admin.html` | لوحة تحكم الأدمن (دخول بالبريد وكلمة المرور) |
| `api.html` | عرض بيانات Firestore بصيغة JSON |
| `check.html` | صفحة فحص التطبيق والاتصال |
| `firestore.rules` | قواعد أمان Firestore |

## ملاحظة مهمة

ملف `firestore.rules` لسه فيه `ADMIN_EMAIL` مكان بريد الأدمن — لازم يتغير للبريد الحقيقي قبل نشر القواعد من Firebase Console، وإلا حساب الأدمن مش هيتعرف.
