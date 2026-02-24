---
created: 2026-02-24
updated: 2026-02-24 15:37
tags:
  - Temporal102
---
# 4-Codebase Architecture (Best Practice)

## 1️⃣ Activities Layer
- فقط شامل Business Logic
- وابسته به context.Context
- مجاز به IO / DB / API Call
- نباید workflow package را import کند

---

## 2️⃣ Workflows Layer
- Orchestration Logic
- Deterministic Code فقط
- استفاده از workflow.Context
- هیچ IO مستقیم مجاز نیست
- فقط Activity صدا می‌زند

---

## 3️⃣ Worker Process
- Glue Layer
- Register کردن Workflow و Activity
- اتصال به Cluster
- Poll کردن Task Queue

---

## 4️⃣ Starter (Client)
- شروع کننده Workflow Execution
- تعیین Workflow ID
- تعیین Task Queue
- ارسال Input

## لینک‌ها


[[Hands-On Exercise ـObserving Durable Execution]]

----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
