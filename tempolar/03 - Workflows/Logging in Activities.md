---
created: 2026-02-20
updated: 2026-02-20 09:43
tags:
  - Temporal102
---
**متن یادداشت:** در بدنه Activity نیز باید از لاگر مخصوص استفاده کرد تا اطلاعات سیستمی مثل `TaskQueue` به صورت خودکار به لاگ اضافه شود.

Go

```
func MyActivity(ctx context.Context, input string) error {
    logger := activity.GetLogger(ctx)
    logger.Info("Executing activity", "input", input)
    // ...
    return nil
}
```

**تفاوت با Workflow:** لاگر Activity نیازی به حذف پیام‌های تکراری در Replay ندارد، اما باعث هماهنگی با سیستم لاگینگ کل اپلیکیشن می‌شود.


لینک ها : 


----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
