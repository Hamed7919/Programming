---
created: 2026-02-24
updated: 2026-02-24 15:35
tags:
  - Temporal102
source:
---
# MOC - Temporal Go Project Structure

برای راه‌اندازی یک سیستم استاندارد Temporal حداقل به ۴ Codebase نیاز داریم:

1. Activities
2. Workflows
3. Worker
4. Starter (Client)

---

Dependency Flow:

Starter → Temporal Cluster  
Worker → Temporal Cluster  
Workflow → Activity  
Activity → External Systems

---

قاعده مهم:

بWorkflow و Activity هرگز مستقیماً همدیگر را import نمی‌کنند.
ارتباط آن‌ها فقط از طریق Worker Registration و نام‌گذاری انجام می‌شود.
## لینک‌ها


[[Hands-On Exercise ـObserving Durable Execution]]

---

## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود


