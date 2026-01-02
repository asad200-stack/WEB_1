# دليل خطوات النشر على Railway - بالعربي

## المشكلة الحالية:
- المتغيرات موجودة في خدمة Postgres (خطأ)
- يجب إضافتها في خدمة WEB_1 (صح)
- البناء فاشل لأن المتغيرات غير موجودة في المكان الصحيح

## الخطوات الصحيحة:

### الخطوة 1: اربط قاعدة البيانات (DATABASE_URL)

1. اذهب إلى خدمة **WEB_1** (اللي فيها أيقونة GitHub)
2. اضغط على تبويب **Variables**
3. اضغط على زر **"Shared Variable"** (السهم البنفسجي)
4. اختر **Postgres**
5. اختر **DATABASE_URL**
6. اضغط OK

هذا سيربط رابط قاعدة البيانات تلقائياً!

---

### الخطوة 2: أضف المتغيرات الأخرى

في نفس صفحة **WEB_1 → Variables**:

اضغط **"+ New Variable"** وأضف كل متغير واحد واحد:

1. **JWT_SECRET** = `QPGscfSCt6Lt7Wcs38zI2g8AMm99Y6EzeMbqSvH2oxk=`
2. **JWT_EXPIRES_IN** = `7d`
3. **NODE_ENV** = `production`
4. **MAX_FILE_SIZE** = `5242880`
5. **UPLOAD_DIR** = `./public/uploads`
6. **PLATFORM_NAME** = `Multi-Store Platform`
7. **SUPER_ADMIN_EMAIL** = `admin@platform.com`
8. **SUPER_ADMIN_PASSWORD** = `admin123456`

---

### الخطوة 3: إعادة النشر (Redeploy)

1. اذهب إلى خدمة **WEB_1**
2. اضغط على تبويب **Deployments**
3. اضغط زر **"Redeploy"**
4. انتظر حتى ينتهي البناء (Build)

---

### الخطوة 4: بعد نجاح البناء

#### 4.1: تشغيل قاعدة البيانات

1. في **WEB_1 → Deployments** → آخر deployment
2. اضغط **"Run Command"**
3. اكتب: `npx prisma migrate deploy`
4. اضغط **"Run"**

#### 4.2: إنشاء حساب المدير

1. في نفس **"Run Command"**
2. اكتب: `node scripts/create-admin.js admin@platform.com admin123456`
3. اضغط **"Run"**

---

### الخطوة 5: تسجيل الدخول

1. اذهب إلى رابط موقعك من Railway
2. اذهب إلى: `/admin/login`
3. أدخل:
   - **Email**: `admin@platform.com`
   - **Password**: `admin123456`

---

## ملخص سريع:

✅ **WEB_1** → Variables → Shared Variable → Postgres → DATABASE_URL
✅ أضف 8 متغيرات إضافية
✅ Redeploy
✅ Run Command: `npx prisma migrate deploy`
✅ Run Command: `node scripts/create-admin.js admin@platform.com admin123456`
✅ سجل دخول!

---

## إذا فشل البناء بعدها:

1. افتح **Deployments** → اقرأ الـ logs
2. تأكد من وجود كل المتغيرات في **WEB_1** (ليس Postgres)
3. تأكد من أن DATABASE_URL مربوط بشكل صحيح

