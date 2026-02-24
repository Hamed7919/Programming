---
created: 2026-02-20
updated: 2026-02-20 11:31
tags:
  - Temporal102
---
# Timer Use Cases

---

# ⏳ Timer Use Cases در Temporal

Timer یعنی:

> به Workflow بگیم "الان صبر کن، بعد ادامه بده"

---

# 1️⃣ اجرای Activity در فواصل مشخص (Reminder Pattern)

مثال ساده:

Customer ثبت‌نام کرده.

می‌خوای:

- 1 روز بعد ایمیل بزنی
    
- 1 هفته بعد ایمیل دوم
    
- 1 ماه بعد ایمیل سوم
    

مدل ساده:

```go
workflow.Sleep(ctx, 24*time.Hour)
workflow.ExecuteActivity(ctx, SendEmail1)

workflow.Sleep(ctx, 7*24*time.Hour)
workflow.ExecuteActivity(ctx, SendEmail2)

workflow.Sleep(ctx, 30*24*time.Hour)
workflow.ExecuteActivity(ctx, SendEmail3)
```

Workflow زنده می‌مونه و طبق زمان‌بندی جلو میره.

---

# 2️⃣ Delay داینامیک (زمان متغیر)

گاهی مدت انتظار از قبل معلوم نیست.

مثلاً:

یه API میگه:

> Retry کن بعد از 17 دقیقه

تو همون مقدار رو استفاده می‌کنی:

```go
delay := response.RetryAfter
workflow.Sleep(ctx, delay)
```

یعنی زمان خواب وابسته به خروجی قبلیه.

---

# 3️⃣ انتظار برای فرآیندهای آفلاین (خیلی مهم)

بعضی چیزها زمان واقعی می‌برن:

- پاس شدن چک بانکی
    
- ارسال مدارک با پست
    
- تأیید دستی کارمند
    

اینجا Workflow ممکنه:

- چند روز
    
- چند هفته
    
- حتی چند ماه
    

صبر کنه.

Temporal برای همین ساخته شده.

---

# 🧠 نکته طراحی خیلی مهم

Workflow باید فرض کنه:

> ممکنه دقیقاً سر زمان مشخص Resume نشه.

چرا؟

چون ممکنه:

- Worker خاموش بوده
    
- سرور maintenance بوده
    
- Task Queue شلوغ بوده
    
- outage اتفاق افتاده
    

---

# 🎯 یعنی چی در عمل؟

اگر نوشتی:

```go
workflow.Sleep(ctx, 24*time.Hour)
```

باید انتظار داشته باشی:

ممکنه 24 ساعت و 10 دقیقه بعد resume بشه.

نه دقیقاً سر 24 ساعت.

---

# 🔥 پس قانون طلایی طراحی

Workflow رو اینطوری طراحی کن:

> زمان‌ها approximate هستن، نه میلی‌ثانیه‌ای دقیق.

Temporal سیستم real-time نیست.  
سیستم reliable orchestration هست.

---

# 🧩 خلاصه خیلی ساده

|کاربرد|چرا Timer استفاده می‌کنیم|
|---|---|
|یادآوری دوره‌ای|اجرای مرحله‌ای با فاصله|
|Retry داینامیک|صبر بر اساس پاسخ قبلی|
|فرآیندهای انسانی/آفلاین|صبر طولانی بدون مصرف CPU|

---

اگر بخوای می‌تونم یه مثال واقعی بانکی طراحی کنم که همه این سه حالت رو با هم داشته باشه تا کامل تو ذهنت قفل شه 👌
----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
