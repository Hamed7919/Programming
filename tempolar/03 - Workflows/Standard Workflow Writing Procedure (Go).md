---
created: 2026-02-24
updated: 2026-02-24 15:41
tags:
  - Temporal102
---

# Procedure - Writing a Production-Ready Workflow

Step 1️⃣
دریافت workflow.Context

Step 2️⃣
تنظیم ActivityOptions
- StartToCloseTimeout (دقیق و نه پیش‌فرض)
- RetryPolicy (در صورت نیاز)

Step 3️⃣
ctx = workflow.WithActivityOptions(ctx, ao)

Step 4️⃣
ExecuteActivity با تابع typed (نه string-based)

ترجیح استاندارد:
workflow.ExecuteActivity(ctx, activities.SampleActivity, input)

نه:
"SampleActivity" (string)

---

Step 5️⃣
استفاده از Timer فقط با:
- workflow.Sleep
- workflow.NewTimer

هرگز از time.Sleep استفاده نشود.

---

Step 6️⃣
Error Propagation شفاف
هیچ error ای swallow نشود.
- 


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
