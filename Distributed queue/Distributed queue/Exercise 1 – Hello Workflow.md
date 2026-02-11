اوکی، گرفتم 👌  
می‌خوای تمرین رو **از صفرِ صفر** بچینیم؛ جوری که انگار فقط:

> «Temporal رو نصب کردم و هیچی دیگه بلد نیستم»

پس من از این‌جا به بعد این فرض‌ها رو می‌گیرم:

- ✅ Temporal CLI نصب شده
    
- ❌ هنوز هیچ Workflow ـی اجرا نکردی
    
- ❌ Worker، Task Queue، Web UI → همش تازه‌ان
    
- 🧠 هدف: _واقعاً بفهمی چی به چیه_ نه فقط کپی‌پیست
    

بریم خیلی تمیز و قدم‌به‌قدم 👇  
(مثل لابراتوار)

---

# Exercise 1 – Hello Workflow

## از باز کردن ترمینال تا اجرای Workflow

---

## Step 0️⃣ – باز کردن ترمینال

روی لینوکس (سیستم خودت):

- `Ctrl + Alt + T`
    

یا هر ترمینالی که دوست داری.

---

## Step 1️⃣ – چک کنیم Temporal نصب شده یا نه

```bash
temporal version
```

اگر خروجی شبیه این دیدی، یعنی اوکیه:

```text
temporal version x.y.z
```

❌ اگر error گرفتی، این تمرین رو نگه می‌داریم و اول نصب رو درست می‌کنیم.

---

## Step 2️⃣ – استارت Temporal Server (خیلی خیلی مهم)

Temporal بدون سرور = هیچ 😄

برای تمرین، ساده‌ترین راه:

```bash
temporal server start-dev
```

چی اتفاق می‌افته؟

- یک **Temporal Server لوکال** بالا میاد
    
- دیتابیس داخلی داره
    
- Web UI هم بالا میاره
    

خروجی‌هایی می‌بینی، آخرش می‌ایسته و اجرا می‌مونه.

❗ این ترمینال رو نبند  
اسمش رو بذار تو ذهنت: **Terminal #1 (Server)**

---

## Step 3️⃣ – باز کردن ترمینال دوم

یک ترمینال جدید باز کن:

```text
Terminal #2
```

این یکی رو برای:

- Worker
    
- CLI commands
    

استفاده می‌کنیم.

---

## Step 4️⃣ – رفتن به پروژه تمرینی

فرض می‌کنیم پروژه تمرینی رو داری (همون که practice داره):

```bash
cd path/to/your/project
cd practice
```

اگر این‌جا بگی ساختارش چیه (Node / Go / Python)، من دقیق‌تر می‌کنم.

---

## Step 5️⃣ – الان یک مکث مفهومی 🧠

الان تو سیستم تو داریم:

```
[ Temporal Server ]  ← روشن
        ↑
   هنوز هیچ Worker نیست
   هنوز هیچ Workflow اجرا نشده
```

پس اگر الان Workflow استارت کنیم → هیچ اتفاقی نمی‌افته.

---

## Step 6️⃣ – اصلاح Worker (اولین کد واقعی)

فایل Worker رو باز کن (مثلاً):

```bash
code .
# یا
nano worker.ts
```

دنبال جایی بگرد که Worker ساخته می‌شه.

قبل (ناقص):

```ts
Worker.create({
  workflowsPath: ...
  activities: ...
})
```

بعد (درست):

```ts
Worker.create({
  workflowsPath: ...
  activities: ...
  taskQueue: "greeting-tasks"
})
```

🔴 این اسم رو حفظ کن:

```text
greeting-tasks
```

---

## Step 7️⃣ – اجرای Worker

هنوز تو **Terminal #2**:

```bash
npm run worker
# یا معادل زبانت
```

خروجی:

```text
Worker started
```

و بعد هیچی…  
این یعنی Worker زنده‌ست و منتظر کاره.

اسم این رو بذار:

> **Terminal #2 = Worker**

---

## Step 8️⃣ – باز کردن ترمینال سوم

بله، سومی 😄

```text
Terminal #3
```

این فقط برای CLI command ـهاست.

---

## Step 9️⃣ – استارت Workflow از CLI

```bash
temporal workflow start \
  --type GreetingWorkflow \
  --task-queue greeting-tasks \
  --workflow-id hello-1 \
  --input '"Hamed"'
```

اگر همه‌چی درست باشه:

- دستور بدون error اجرا می‌شه
    
- Workflow استارت می‌شه
    
- Worker اون رو پردازش می‌کنه
    

---

## Step 🔟 – الان چی داریم؟

```
Terminal #1 → Temporal Server ✅
Terminal #2 → Worker (polling greeting-tasks) ✅
Terminal #3 → Workflow started ✅
```

ولی:  
❓ خروجی کجاست؟

عمداً نمی‌بینیمش 😉

---

## Step 1️⃣1️⃣ – دیدن نتیجه (Web UI)

مرورگر رو باز کن:

```
http://localhost:8233
```

می‌بینی:

- Workflow ID: `hello-1`
    
- Status: COMPLETED
    
- Result:
    
    ```
    Hello, Hamed!
    ```
    

🎉🎉🎉

---

## جمع‌بندی خیلی خلاصه

تو این تمرین یاد گرفتی:

- Temporal بدون Server کار نمی‌کنه
    
- Worker باید روی Task Queue گوش بده
    
- Workflow از CLI استارت می‌شه
    
- Web UI برای دیدن نتیجه‌ست
    

---

قدم بعدی پیشنهادی من:  
1️⃣ عمداً Task Queue رو اشتباه کنیم  
2️⃣ نتیجه رو از CLI بگیریم نه UI  
3️⃣ بفهمیم Workflow ID چرا مهمه

بگو بعدی رو کدومو بریم 👇