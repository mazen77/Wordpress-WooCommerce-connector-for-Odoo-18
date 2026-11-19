# WooCommerce and Odoo 18 installation guide

Extract the outer download once. It contains two software packages and two language guides.

## Install on WordPress

1. Sign in to the staging WordPress dashboard. Activate WooCommerce first. The connector requires WordPress 6.5+, WooCommerce 8.2+ and PHP 8.0+.
2. Open **Plugins → Add New Plugin → Upload Plugin**. Select **WordPress-Plugin-2.1.6.zip** and keep it zipped.
3. Click **Install Now**, then **Activate Plugin**. Open **WooCommerce → Odoo Sync**.

## Install on Odoo 18

Use a fresh staging database. The previous connector and this module share database model names and cannot be installed together.

1. Extract **Odoo-18-Module-18.0.1.0.0.zip**. It contains one folder named `woo_commerce_connector`. Keep the folder name and its contents together.
2. **Odoo.sh** — Commit that folder at the root of the GitHub branch connected to staging. Confirm the repository shows `woo_commerce_connector/__manifest__.py`. Wait for the build to succeed and open the staging database.
   
   **Self-hosted Odoo** — Copy the same folder into a directory listed in `addons_path`. Ensure the Odoo Python environment has Requests and Pillow, then restart Odoo.
3. In Odoo, enable developer mode. Under **Apps**, choose **Update Apps List** and confirm. Search for **WooCommerce Connector (Odoo 18 Module)** and click **Activate**.
4. Grant the setup administrator the **Woo Connector Manager** group. Open **WooCommerce → Connections**.

## Connect and test

1. In WooCommerce → Odoo Sync, copy the store identifier and set the Odoo event URL to https://odoo.example.com/mw-sync/v2/event. In Odoo → WooCommerce → Connections, use the same identifier and set the stock URL to https://shop.example.com/wp-json/mw-sync/v2/stock. Replace the example hosts with yours. Match the outgoing secret on each side to the incoming secret on the other. Use two different random secrets of at least 32 characters. Keep them out of Git.
2. Assign a dedicated internal Odoo user the Woo Connector Service group and access to the intended company. Configure the warehouse, pricelist, tax mappings, payment journals and shipping/fee service product. Configure the order cutover before enabling live sync. Leave automatic fulfillment blank unless a particular Woo status should validate ready deliveries and trigger invoicing.
3. Enable event sync and test a product with a unique SKU, a variation and a new order. Check quantities, pricing, tax and the reservation acknowledgement in both systems. Enable stock publishing after the mapping and warehouse quantities agree. Test cancellations and refunds on staging before enabling production order processing.



# دليل تثبيت

فك ضغط ملف التحميل الرئيسي مرة واحدة. ستجد حزمتين للتثبيت ودليلًا لكل لغة.

## تثبيت إضافة ووردبريس

1. سجّل الدخول إلى لوحة تحكم المتجر التجريبي وفعّل WooCommerce. تتطلب الإضافة ووردبريس 6.5 أو أحدث، وWooCommerce 8.2 أو أحدث، وPHP 8.0 أو أحدث.
2. افتح **إضافات** ثم **أضف إضافة جديدة** ثم **رفع إضافة**. اختر الملف **WordPress-Plugin-2.1.6.zip** واتركه مضغوطًا.
3. اضغط **التنصيب الآن** ثم **تفعيل الإضافة**. ستجد **Odoo Sync** في قائمة **WooCommerce**.

## تثبيت وحدة أودو 18

ابدأ بقاعدة بيانات تجريبية جديدة. تتشارك هذه الوحدة مع الموصل السابق أسماء نماذج البيانات، ولذلك يتعذّر تثبيتهما معًا في القاعدة نفسها.

1. فك ضغط **Odoo-18-Module-18.0.1.0.0.zip**. ستجد مجلدًا واحدًا اسمه `woo_commerce_connector`. احتفظ باسمه وبجميع الملفات داخله.
2. **على Odoo.sh**، أضف المجلد إلى جذر فرع GitHub المرتبط بالبيئة التجريبية. تأكّد من ظهور الملف في المسار `woo_commerce_connector/__manifest__.py`. بعد نجاح البناء، افتح قاعدة البيانات التجريبية.

   **على خادم أودو خاص**، انسخ المجلد إلى أحد المسارات المحددة في `addons_path`. تأكّد من توفر مكتبتي Requests وPillow في بيئة Python التي يستخدمها أودو، ثم أعد تشغيل خدمة أودو.
3. فعّل وضع المطوّر في أودو. افتح **التطبيقات** واختر **تحديث قائمة التطبيقات** وأكّد التحديث. ابحث عن **WooCommerce Connector (Odoo 18 Module)** ثم اضغط **تفعيل**.
4. امنح مسؤول الإعداد صلاحية **Woo Connector Manager**. افتح قائمة **WooCommerce** ثم **Connections**.

## ربط النظامين واختبار المزامنة

1. من **WooCommerce → Odoo Sync**، انسخ معرّف المتجر وأدخل عنوان استقبال الأحداث في Odoo. ثم افتح **WooCommerce → Connections** داخل Odoo، وأدخل المعرّف نفسه وعنوان استقبال المخزون في المتجر. استبدل نطاقي المثال بنطاقيك الفعليين.

   | الإعداد | مثال |
   | --- | --- |
   | عنوان الأحداث في إعدادات WooCommerce | `https://odoo.example.com/mw-sync/v2/event` |
   | عنوان المخزون في إعدادات Odoo | `https://shop.example.com/wp-json/mw-sync/v2/stock` |

   أنشئ مفتاحين عشوائيين مختلفين، طول كل منهما 32 محرفًا على الأقل. يجب أن يطابق مفتاح الإرسال في كل نظام مفتاح الاستقبال في النظام الآخر. احتفظ بالمفتاحين خارج مستودع Git.
2. خصّص مستخدمًا داخليًا في Odoo للربط، وأضفه إلى مجموعة **Woo Connector Service** وامنحه صلاحية الوصول إلى الشركة المطلوبة. حدّد المستودع وقائمة الأسعار ومطابقة الضرائب ويوميات الدفع ومنتج الخدمة المستخدم للشحن والرسوم. اضبط تاريخ بدء معالجة الطلبات قبل تشغيل المزامنة المباشرة. اترك حقل **Automatic fulfillment status** فارغًا إلا إذا أردت أن تؤدي حالة طلب محددة في WooCommerce إلى اعتماد عمليات التسليم الجاهزة وإصدار الفواتير.
3. فعّل مزامنة الأحداث، ثم اختبر منتجًا له رمز صنف فريد (SKU)، وأحد متغيرات المنتجات، وطلبًا جديدًا. راجع الكميات والأسعار والضرائب وتأكيد حجز المخزون في النظامين. فعّل إرسال المخزون بعد التحقق من مطابقة المنتجات والكميات في المستودع. اختبر الإلغاء والاسترداد في البيئة التجريبية قبل تفعيل معالجة الطلبات في المتجر الفعلي.
