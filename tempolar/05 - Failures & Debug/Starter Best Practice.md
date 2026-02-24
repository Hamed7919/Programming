---
created: 2026-02-24
updated: 2026-02-24 15:47
tags:
  - "#Temporal102"
---

#  # Starter Design Rule

1️⃣ Workflow ID باید Unique باشد (یا intentional reuse)

2️⃣ از ID ثابت در Production استفاده نکن
مگر اینکه قصد idempotency داری

مثال بهتر:

ID: fmt.Sprintf("durable-%d", time.Now().Unix())

3️⃣ TaskQueue باید با Worker یکی باشد


لینک ها :
‌[[Hands-On Exercise ـObserving Durable Execution]]

---

## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
