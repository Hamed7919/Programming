---
created: 2026-02-24
updated: 2026-02-24 15:40
tags:
  - Temporal102
source:
---
# Deterministic Boundary Rule

اصل طلایی Temporal:

Workflow Code باید 100% Deterministic باشد.

مجاز نیست:
- time.Now()
- rand.Int()
- HTTP Call
- DB Query

همه اینها باید داخل Activity باشند.

---

مرز سیستم:

Workflow = Orchestrator  
Activity = Side Effects
## لینک‌ها


[[Hands-On Exercise ـObserving Durable Execution]]

---

## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود


