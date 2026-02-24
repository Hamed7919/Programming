---
created: 2026-02-24
updated: 2026-02-24 15:43
tags:
  - Temporal102
---
# Standard Timer Pattern

1️⃣ تعریف واضح دلیل Timer
- Retry Delay؟
- Business Reminder؟
- Human Wait؟

2️⃣ تعیین Duration منطقی
- نه کمتر از 1 ثانیه
- متناسب با SLA

3️⃣ Robust بودن در برابر Outage

Workflow باید فرض کند:
ممکن است Resume شدن بسیار دیرتر رخ دهد.

پس:
هیچ منطق وابسته به "دقیق بودن زمان" ننویس.


لینک ها : 

[[Hands-On Exercise ـObserving Durable Execution]]

----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
