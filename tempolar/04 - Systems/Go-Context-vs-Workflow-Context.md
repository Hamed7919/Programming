---
created: 2026-02-19
updated: 2026-02-19 12:21
tags:
  - Temporal102
---

# {{Go-Context-vs-Workflow-Context}} 
|**ویژگی**|**context.Context (Go)**|**workflow.Context (Temporal)**|
|---|---|---|
|**منبع حقیقت**|ساعت و سیستم‌عامل ورکر|تاریخچه رویدادها (Event History)|
|**قابلیت Replay**|ندارد (در هر بار اجرا متفاوت است)|دارد (در ری‌پلی، گذشته را بازسازی می‌کند)|
|**عملیات مجاز**|HTTP, DB, Disk I/O|فقط صدا زدن Activity و Timerهای تمپورال|
|**تزریق مقادیر**|`context.WithValue`|`workflow.WithValue`|
|**تایم‌اوت**|`context.WithTimeout` (واقعی)|`workflow.WithActivityOptions` (منطقی)|

## لینک‌ها


[[Temporal-Context-Dualism]]

----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
