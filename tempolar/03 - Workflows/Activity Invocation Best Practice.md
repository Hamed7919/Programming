---
created: 2026-02-24
updated: 2026-02-24 15:45
tags:
  - Temporal102
---
# Activity Invocation Best Practice

به جای string-based registration:

بد:
workflow.ExecuteActivity(ctx, "SampleActivity", name)

خوب:
workflow.ExecuteActivity(ctx, activities.SampleActivity, name)

مزایا:
- Type Safety
- Compile-time check
- Refactor-friendly


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
