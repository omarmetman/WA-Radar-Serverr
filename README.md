```markdown
# 🛡️ WA-Radar-Server

> رادار الرسائل المحذوفة في واتساب - راقب رسائلك وأعلم فورًا عند حذف أي رسالة

<br>

<div align="center">

![Node.js](https://img.shields.io/badge/Node.js-18+-green?style=for-the-badge&logo=node.js)
![WhatsApp](https://img.shields.io/badge/WhatsApp-Web.js-25D366?style=for-the-badge&logo=whatsapp)
![Telegram](https://img.shields.io/badge/Telegram-Bot-26A5E4?style=for-the-badge&logo=telegram)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker)

</div>

<br>

## 📖 عن المشروع

**WA-Radar-Server** هو سيرفر خلفي يتصل بحساب واتساب الخاص بك ويراقب جميع الرسائل النصية الواردة. 

### 🎯 آلية العمل

عندما يقوم أي شخص بحذف رسالة نصية ("حذف للجميع")، يقوم السيرفر فورًا بإرسال تفاصيل الرسالة المحذوفة إلى بوت تليجرام خاص بك:

```
🚨 Deleted Message Detected!
👤 Sender: أحمد محمد
📩 Message: رسالة سرية لا يجب أن تراها
🕒 Time: 10:30:45 PM
```

> **⚠️ ملاحظة:** الإصدار الحالي يدعم **الرسائل النصية فقط** (الملفات والصور غير مدعومة حاليًا)

<br>

## ✨ المميزات

| الميزة | الوصف |
|--------|-------|
| 🔍 **مراقبة فورية** | يكتشف الرسائل فور وصولها |
| 🗑️ **كشف الحذف** | يلتقط أي رسالة يتم حذفها للجميع |
| 📨 **تنبيهات تليجرام** | يرسل التفاصيل مباشرة لحسابك |
| 💾 **جلسة محفوظة** | لا حاجة لمسح QR code في كل مرة |
| 🐳 **دعم Docker** | تشغيل 24/7 بكل سهولة |
| 🚀 **خفيف وسريع** | لا يستهلك موارد كبيرة |

<br>

## 🛠️ التقنيات المستخدمة

<div align="center">

| التقنية | الاستخدام |
|:--------|:----------|
| **Node.js** | بيئة التشغيل الأساسية |
| **whatsapp-web.js** | التفاعل مع واتساب ويب |
| **Puppeteer** | محاكاة المتصفح الخفي |
| **Telegram Bot API** | إرسال التنبيهات |
| **QRCode** | إنشاء رموز QR |
| **Docker** | التغليف والتشغيل |

</div>

<br>

## 📋 المتطلبات الأساسية

قبل البدء، تأكد من توفر:

- ✅ [Node.js](https://nodejs.org/) (الإصدار 18 أو أحدث)
- ✅ حساب بوت تليجرام + `TG_TOKEN` (من [@BotFather](https://t.me/BotFather))
- ✅ معرف الدردشة `TG_CHAT_ID` (من [@userinfobot](https://t.me/userinfobot))

<br>

## 🚀 طريقة التشغيل

### 📦 الطريقة الأولى: التشغيل المباشر

```bash
# 1. استنساخ المشروع
git clone https://github.com/omarmetman/WA-Radar-Serverr.git
cd WA-Radar-Serverr

# 2. تثبيت الاعتماديات
npm install

# 3. إنشاء ملف .env وإضافة التوكنات
echo "TG_TOKEN=توكن_البوت_الخاص_بك" > .env
echo "TG_CHAT_ID=معرف_الدردشة" >> .env

# 4. تشغيل السيرفر
npm start
```

### 🐳 الطريقة الثانية: التشغيل باستخدام Docker (الأفضل للتشغيل 24/7)

**الملف الأول: `docker-compose.yml`**
```yaml
version: '3.8'

services:
  wa-radar:
    build: .
    container_name: wa-radar
    restart: unless-stopped
    environment:
      - TG_TOKEN=${TG_TOKEN}
      - TG_CHAT_ID=${TG_CHAT_ID}
    volumes:
      - ./whatsapp-session:/app/.wwebjs_auth
      - ./puppeteer-cache:/app/.wwebjs_cache
    stdin_open: true
    tty: true
```

**الملف الثاني: `Dockerfile`**
```dockerfile
FROM node:18-slim

RUN apt-get update && apt-get install -y \
    ca-certificates \
    fonts-liberation \
    libasound2 \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libcups2 \
    libdbus-1-3 \
    libgbm1 \
    libgtk-3-0 \
    libnspr4 \
    libnss3 \
    libx11-xcb1 \
    libxcomposite1 \
    libxdamage1 \
    libxrandr2 \
    --no-install-recommends \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY index.js ./
RUN mkdir -p /app/.wwebjs_auth /app/.wwebjs_cache

CMD ["node", "index.js"]
```

**أمر التشغيل:**
```bash
docker-compose up -d
```

<br>

## 🔗 ربط حساب واتساب

بعد تشغيل السيرفر، اتبع الخطوات التالية:

| الخطوة | الإجراء |
|:------:|---------|
| 1️⃣ | افتح تليجرام - ستصل إليك صورة بها رمز QR |
| 2️⃣ | افتح واتساب على هاتفك |
| 3️⃣ | اذهب إلى **الإعدادات ← الأجهزة المرتبطة ← ربط جهاز** |
| 4️⃣ | امسح رمز QR من شاشة هاتفك |
| 5️⃣ | 🎉 تم! السيرفر الآن في وضع المراقبة |

<br>

## 📁 هيكل المشروع

```
WA-Radar-Server/
├── 📄 index.js              # الكود الرئيسي للتطبيق
├── 📄 package.json          # قائمة الاعتماديات
├── 📄 package-lock.json     # إصدارات محددة للاعتماديات
├── 🐳 docker-compose.yml    # تكوين Docker
├── 🐳 Dockerfile           # بناء صورة Docker
├── 🔒 .env                 # متغيرات البيئة (سري - لا ترفعه)
├── 🚫 .gitignore           # الملفات المستثناة من Git
└── 📁 .wwebjs_auth/        # جلسة واتساب المحفوظة (تتولد تلقائياً)
```

<br>

## 🔧 أوامر مفيدة

| الأمر | الوصف |
|-------|-------|
| `npm start` | تشغيل السيرفر |
| `npm install` | تثبيت الاعتماديات |
| `Ctrl + C` | إيقاف السيرفر |
| `docker-compose up -d` | تشغيل Docker في الخلفية |
| `docker-compose down` | إيقاف Docker |
| `docker logs -f wa-radar` | مشاهدة السجلات |

<br>

## ❓ الأسئلة الشائعة

<details>
<summary><b>❓ كيف يعرف السيرفر الرسائل المحذوفة؟</b></summary>

السيرفر يخزن كل رسالة تصل في ذاكرة مؤقتة. عندما يكتشف حدث "حذف للجميع"، يرجع إلى الذاكرة ويستعيد نص الرسالة الأصلية ويرسلها لك.
</details>

<details>
<summary><b>❓ هل يخزن السيرفر رسائلي بشكل دائم؟</b></summary>

لا، يتم تخزين آخر 500 رسالة فقط في الذاكرة العشوائية (RAM). عند إعادة تشغيل السيرفر، تفقد جميع الرسائل المخزنة.
</details>

<details>
<summary><b>❓ هل يدعم الصور والفيديوهات؟</b></summary>

حالياً لا، الإصدار الحالي يدعم النصوص فقط. قد نضيف هذه الميزة في المستقبل.
</details>

<details>
<summary><b>❓ هل هذا آمن؟</b></summary>

نعم، الكود مفتوح المصدر وشفاف تماماً. أنت تتحكم في السيرفر بنفسك وتستطيع مراجعة الكود قبل تشغيله.
</details>

<br>

## ⚠️ إخلاء مسؤولية

<div align="center">

> ⚠️ **تنبيه مهم**

</div>

- هذا المشروع للأغراض **التعليمية والبحثية** فقط
- استخدامه قد ينتهك **شروط خدمة واتساب**
- **أنت وحدك تتحمل المسؤولية الكاملة** عن استخدامه
- لا يُستخدم للتجسس على الآخرين أو انتهاك خصوصيتهم

<br>

## 👨‍💻 المطور

<div align="center">

### **Omar M. Etman**

[![Website](https://img.shields.io/badge/الموقع_الرسمي-omarmetman.vercel.app-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://omarmetman.vercel.app/)
[![GitHub](https://img.shields.io/badge/GitHub-@omarmetman-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/omarmetman)

</div>

<br>

## 📄 الترخيص

<div align="center">

```
جميع الحقوق محفوظة © 2024 Omar M. Etman

هذا المشروع مرخص تحت رخصة MIT
يمكنك استخدامه وتعديله وتوزيعه بحرية مع الإشارة إلى حقوق الملكية
```

</div>

<br>

---

<div align="center">

### ⭐ لا تنسى وضع نجمة على المشروع إذا أعجبك ⭐

**مصنوع بحب ❤️ للمجتمع مفتوح المصدر**

</div>
```
