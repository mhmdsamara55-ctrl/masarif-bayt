# 🏠 مصاريف البيت - Masarif Al-Bayt

تطبيق ويب لإدارة مصاريف العيلة مع الزوج/الزوجة بمزامنة فورية عبر Firebase

---

## ✨ الميزات

✅ **Google Sign-in آمن**  
✅ **عيلات متعددة معزولة**  
✅ **إدارة المصاريف والدخل**  
✅ **دعم عملات متعددة**  
✅ **تصميم RTL عربي**  
✅ **Firestore Security محترم**  

---

## 🚀 الشروع السريع

### المتطلبات
- Firebase Project (جاهز بالفعل ✅)
- Google OAuth Client ID (جاهز بالفعل ✅)
- GitHub Account

---

## 📋 الخطوات الأخيرة

### 1️⃣ إنشاء Repository على GitHub

```bash
# إذا كان عندك Git مثبت
git init masarif-bayt
cd masarif-bayt
```

أو انشئ repository مباشرة على GitHub:
1. ذهب إلى https://github.com/new
2. اسم Repository: `masarif-bayt`
3. **Create repository**

---

### 2️⃣ رفع الملفات

```bash
# نسخ الملفات للمجلد
cp masarif-bayt-v2.html index.html
cp SETUP_GUIDE_AR.md .
cp MIGRATION_PLAN_AR.md .
cp README.md .

# إذا كنت في مجلد فارغ
git init
git add .
git commit -m "feat: Masarif Al-Bayt v2.0 with Google Sign-in and Firestore Security"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/masarif-bayt.git
git push -u origin main
```

---

### 3️⃣ فعّل GitHub Pages

1. في Repository → **Settings** → **Pages**
2. **Source:** `main` branch و folder `/ (root)`
3. **Save**
4. انتظر دقيقة أو اثنتين

---

### 4️⃣ الرابط النهائي

```
https://mhmdsamara55-ctrl.github.io/masarif-bayt
```

---

## 🔒 الأمان

✅ **Firestore Security Rules** محترم  
✅ **Google Authentication** مفعّل  
✅ **بيانات مشفّرة** عند النقل  
✅ **معزول لكل عيلة** في Firestore  

---

## 🛠️ الملفات المتضمنة

| الملف | الوصف |
|------|--------|
| `index.html` | التطبيق الكامل (v2.0) |
| `SETUP_GUIDE_AR.md` | دليل الإعداد بالعربية |
| `MIGRATION_PLAN_AR.md` | خطة الانتقال والمهام |
| `README.md` | هذا الملف |

---

## 📱 الاستخدام

### تسجيل الدخول
1. افتح التطبيق
2. اضغط "Sign in with Google"
3. اختر حسابك

### إنشاء عيلة
1. اضغط "➕ إنشاء عيلة جديدة"
2. أدخل اسم العيلة
3. اضغط "إنشاء العيلة"

### إضافة معاملة
1. اختر العيلة
2. ملى المعلومات (الوصف، المبلغ، العملة)
3. اضغط "إضافة"

### دعوة الزوج/الزوجة
1. شارك الكود من "📩 ادعُ الزوج/الزوجة"
2. الزوج/الزوجة يستخدم نفس الكود

---

## 🔧 المتطلبات التقنية

- **Firebase:** Firestore + Authentication
- **Google:** OAuth 2.0 Client ID
- **Browser:** Chrome/Firefox/Safari (آخر إصدار)
- **Network:** اتصال إنترنت

---

## 📊 النسخة

**Version:** 2.0  
**Release Date:** يوليو 2026  
**Status:** 🟢 جاهز للاستخدام  

---

## 🚀 الميزات القادمة

- [ ] نظام الميزانية الشهرية
- [ ] إدارة القروض
- [ ] تقارير PDF
- [ ] Push Notifications
- [ ] Offline Mode
- [ ] Dark Mode

---

## 📞 الدعم

في حالة وجود مشكلة:
1. افتح Browser Console (F12)
2. شوف الـ error message
3. اقرأ SETUP_GUIDE_AR.md للـ Troubleshooting

---

## 📄 الترخيص

هذا المشروع مفتوح المصدر ومتاح للاستخدام الشخصي والتجاري.

---

**Made with ❤️ for families  
Firebase + Google Auth + Firestore Security**
