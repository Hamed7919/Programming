---
created: 2026-02-20
updated: 2026-02-20 10:41
tags:
  - Temporal102
source:
---
# Activity Start-to-Close Timeout

## تعریف
بStart-to-Close Timeout مدت زمانی است که برای اجرای یک Activity Task مجاز در نظر گرفته می‌شود.

این تایم‌اوت مشخص می‌کند:
بTemporal چه مدت منتظر بماند تا Worker اجرای Activity را کامل کند.

---

## چرا مهم است؟

این مقدار دو نقش کلیدی دارد:

1. ⏱ کنترل حداکثر زمان اجرای موفق Activity
2. 💥 تشخیص Crash شدن Worker

---

## تنظیم صحیح چگونه است؟

باید کمی بیشتر از کندترین اجرای موفقی که انتظار داریم تنظیم شود.

نه خیلی کوتاه ❌  
نه خیلی بلند ❌  

بلکه کمی بیشتر از worst-case موفق ✅

---

## اگر خیلی کوتاه باشد

- Activity به Timeout می‌خورد
- Retry Policy فعال می‌شود
- Load سیستم بالا می‌رود
- رفتار غیرمنتظره در Production ایجاد می‌شود

مثال اشتباه:
Hello World → 5 ثانیه
اما بعداً Activity تبدیل می‌شود به:
- Call به Remote Service
- پردازش فایل
- Query دیتابیس

و Timeout آپدیت نمی‌شود ❌

---

## اگر خیلی بلند باشد

- تشخیص Crash شدن Worker دیر انجام می‌شود
- Recovery کند می‌شود
- Throughput سیستم کاهش پیدا می‌کند

Temporal از Start-to-Close Timeout برای تشخیص Worker Failure استفاده می‌کند.

---

## اصل طراحی

Start-to-Close Timeout ≈ (Max Successful Execution Time) + مقدار buffer کوچک

---

## Insight

Timeoutها فقط محدودیت زمانی نیستند،
بلکه بخشی از مکانیزم Failure Detection در Temporal هستند.

...

## لینک‌ها

[[Timeout Misconfiguration Failure Pattern]]


---

## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود


