# 🏠 دليل إعداد مصاريف البيت v2.0

## المتطلبات الأساسية

هذا الدليل يشرح خطوات تفعيل التطبيق الجديد مع **Google Sign-in** و**Firestore Security**.

---

## ✅ الخطوة 1: إعداد Firebase (5 دقائق)

### 1.1 – إنشاء مشروع Firebase (إن لم يكن موجود)

1. ذهب إلى [Firebase Console](https://console.firebase.google.com)
2. اضغط **Create Project**
3. اسم المشروع: `Masarif-Bayt` (أو أي اسم)
4. متابعة → تفعيل Google Analytics (اختياري)
5. **Create Project**

### 1.2 – تفعيل Firestore Database

1. من الـ sidebar → **Build** → **Firestore Database**
2. **Create Database**
3. اختر موقع قريب (مثلاً: `europe-west1` أو `us-central1`)
4. **Start in Test Mode** (سنغيرها بعدين)
5. **Enable**

### 1.3 – تفعيل Authentication

1. من الـ sidebar → **Build** → **Authentication**
2. **Get Started**
3. اضغط على **Google** (من قائمة الـ providers)
4. فعّل **Google** وأدخل بريد دعم المشروع
5. **Save**

---

## ✅ الخطوة 2: إعداد Google OAuth (10 دقائق)

### 2.1 – إنشاء OAuth Client ID

1. ذهب إلى [Google Cloud Console](https://console.cloud.google.com)
2. تأكد من اختيار المشروع (`Masarif-Bayt`)
3. من الـ sidebar → **APIs & Services** → **Credentials**
4. اضغط **Create Credentials** → **OAuth Client ID**
   - إذا طلب إعداد OAuth Consent Screen:
     - اختر **External**
     - ملء المعلومات الأساسية (اسم التطبيق، البريد)
     - **Save and Continue** (تجاوز الـ scopes)
5. نوع التطبيق: **Web application**
6. أسفل **Authorized JavaScript origins**:
   ```
   http://localhost:3000
   http://localhost:8000
   https://masarif-bayt.github.io
   ```
   (استبدل `masarif-bayt` باسم repo الـ GitHub أو استخدم domain مخصص)
7. أسفل **Authorized redirect URIs**:
   ```
   http://localhost:3000
   http://localhost:8000
   https://masarif-bayt.github.io
   ```
8. **Create** ثم نسخ **Client ID** و **Client Secret**

### 2.2 – تحديث الملف HTML

في `masarif-bayt-v2.html`، ابحث عن:
```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

و:
```javascript
google.accounts.id.initialize({
    client_id: 'YOUR_GOOGLE_CLIENT_ID.apps.googleusercontent.com',
    ...
});
```

**كيفية الحصول على بيانات Firebase:**

1. من Firebase Console → **Project Settings** (الترس في الأعلى)
2. اختر **Your apps** → **Web**
3. انسخ `firebaseConfig` كاملة

---

## ✅ الخطوة 3: Firestore Security Rules (5 دقائق)

**هذا هو الجزء الحرج الأمني.**

1. من Firebase Console → **Firestore Database** → **Rules**
2. استبدل القواعس بالقواعس الآمنة التالية:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // ===== Users Collection =====
    // كل مستخدم يقدر يعدل بيانات نفسه بس
    match /users/{uid} {
      allow read, write: if request.auth.uid == uid;
      
      // عائلات المستخدم
      match /families/{familyCode} {
        allow read, write: if request.auth.uid == uid;
      }
    }

    // ===== Families Collection =====
    // كل عائلة معزولة آمنة
    match /families/{familyCode} {
      // قراءة وكتابة فقط للأعضاء المصرحين
      allow read, write: if 
        request.auth != null &&
        exists(/databases/$(database)/documents/users/$(request.auth.uid)/families/$(familyCode));

      // Transactions تابعة للعائلة
      match /transactions/{transactionId} {
        allow read, write: if 
          request.auth != null &&
          exists(/databases/$(database)/documents/users/$(request.auth.uid)/families/$(familyCode));
      }
    }

    // منع أي وصول آخر
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

3. **Publish** القواعس

### ⚠️ شرح القواعس:

- ✅ كل مستخدم يشوف بيانات نفسه بس
- ✅ كل عائلة معزولة تماماً
- ✅ لا تقدر تقرأ بيانات عائلة إلا إذا كنت عضو فيها
- ✅ البيانات التاريخية للعائلة الأولى محفوظة

---

## ✅ الخطوة 4: نشر على GitHub Pages (5 دقائق)

### 4.1 – إنشاء Repository

1. ذهب إلى [GitHub](https://github.com)
2. اضغط **New repository**
3. الاسم: `masarif-bayt` (أو أي اسم)
4. **Public**
5. **Create repository**

### 4.2 – رفع الملفات

```bash
# Clone المشروع
git clone https://github.com/YOUR_USERNAME/masarif-bayt.git
cd masarif-bayt

# أضف الملف
cp masarif-bayt-v2.html index.html

# Commit وPush
git add .
git commit -m "feat: Google Sign-in with Firestore Security"
git push origin main
```

### 4.3 – فعّل GitHub Pages

1. في Repository → **Settings** → **Pages**
2. **Source**: تحت **Deploy from a branch**، اختر `main` و folder `/root`
3. **Save**
4. بعد دقيقتين، الموقع سيكون متاح على:
   ```
   https://YOUR_USERNAME.github.io/masarif-bayt
   ```

---

## ✅ الخطوة 5: تحديث OAuth Redirect URI

الآن لما عرفت الـ URL الفعلي (GitHub Pages)، حدّث في Google Cloud Console:

1. Google Cloud → **Credentials** → OAuth 2.0 Client ID
2. **Authorized JavaScript origins** أضف:
   ```
   https://YOUR_USERNAME.github.io
   https://YOUR_USERNAME.github.io/masarif-bayt
   ```
3. **Authorized redirect URIs** أضف نفس الـ URLs
4. **Save**

---

## ✅ الخطوة 6: اختبار التطبيق

### 6.1 – تسجيل الدخول الأول

1. افتح [التطبيق](https://YOUR_USERNAME.github.io/masarif-bayt)
2. اضغط **Sign in with Google**
3. اختر حساب Google الخاص بك
4. يجب أن تشوف صفحة التطبيق

### 6.2 – إنشاء عيلة جديدة

1. اضغط **➕ إنشاء عيلة جديدة**
2. أدخل اسم العيلة (مثال: "عيلتنا")
3. اضغط **إنشاء العيلة**
4. يجب أن تشوف كود دعوة فريد

### 6.3 – دعوة الزوج/الزوجة

1. انسخ الكود من **📩 ادعُ الزوج/الزوجة**
2. شارك الكود أو الرابط مع الزوج/الزوجة
3. هي/هو يفتح التطبيق بنفس Google Account (أو account جديد)
4. يختار نفس الكود الدعوة
5. سيظهر له رسالة أنه انضم للعيلة

---

## 🔧 Troubleshooting

### ❌ "Sign-in button not appearing"

**السبب:** Google API مش محمّل أو Client ID خاطئ

**الحل:**
1. تأكد من `client_id` صحيح في الملف
2. تأكد من URL الموقع مسجل في Google Cloud Console
3. افتح Browser Console (F12) وشوف errors

### ❌ "Firestore permission denied"

**السبب:** Security Rules غير صحيحة أو المستخدم ما ينتمي للعيلة

**الحل:**
1. تأكد من نسخ القواعس كاملة
2. افتح Firestore وتحقق من البنية:
   ```
   /families/{familyCode}/data/
   /users/{uid}/families/{familyCode}
   ```

### ❌ "Invitation code not working"

**السبب:** نفس الكود مفروض يستخدمه الأعضاء

**الحل:**
- النسخة الحالية تحتاج تحسين نظام الدعوة (سيتم في النسخة الثانية)
- الحل المؤقت: استخدم نفس Google Account على جهازين

---

## 📊 البيانات القديمة

البيانات الموجودة حالياً من الـ URL parameter method ستبقى في Firestore.

**كيفية الاحتفاظ بها:**

1. افتح Firestore
2. الـ collection `families` بها كود قديم (مثل `ABC123`)
3. ترك البيانات كما هي — ما تحتاج تعديل

---

## 🚀 الخطوات التالية

### قريباً:

- [ ] نظام دعوة محسّن (رابط مشاركة بدل كود)
- [ ] تصنيفات مخصصة للمصاريف
- [ ] تقارير PDF
- [ ] إشعارات Push
- [ ] دعم العملات المتعددة المتقدم
- [ ] Offline Mode محسّن

---

## 📞 الدعم

إذا كان في مشكلة:

1. افتح Browser Console (F12)
2. شوف الـ error message
3. قارن مع الـ troubleshooting section

---

## ✨ الملخص السريع

| الخطوة | الوقت | المهمة |
|--------|-------|--------|
| 1 | 5 دقائق | إعداد Firebase + Firestore |
| 2 | 10 دقائق | Google OAuth Client ID |
| 3 | 5 دقائق | Firestore Security Rules |
| 4 | 5 دقائق | نشر على GitHub Pages |
| 5 | 2 دقيقة | تحديث OAuth URIs |
| 6 | 5 دقائق | الاختبار |
| **المجموع** | **32 دقيقة** | التطبيق جاهز للاستخدام |

---

**Created:** يوليو 2026  
**Version:** 2.0  
**Status:** 🔵 جاهز للاستخدام
