---
created: 2026-02-20
updated: 2026-02-20 09:41
tags:
  - Temporal102
---
**متن یادداشت:** برای لاگ گرفتن داخل یک Workflow Definition:

1. **دسترسی به لاگر:** استفاده از `workflow.GetLogger(ctx)`.
    
2. **استفاده از Key-Value:** پیشنهاد می‌شود از ساختار کلید-مقدار برای جستجوی بهتر استفاده کنید.
    

Go

```
// دسترسی به لاگر مخصوص ورک‌فلو
logger := workflow.GetLogger(ctx)
logger.Info("Starting order processing", "OrderID", orderID)

// اضافه کردن فیلدهای ثابت به لاگر
import "go.temporal.io/sdk/log"
logger = log.With(logger, "user_id", "123")
logger.Debug("Checking inventory") // شامل user_id هم خواهد بود
```
لینک ها : 


----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
