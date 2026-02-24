---
created: 2026-02-20
updated: 2026-02-20 09:55
tags:
  - Temporal102
---
**متن یادداشت:** بسته به اینکه چطور از متد `.Get()` استفاده کنید، رفتار Workflow شما متفاوت خواهد بود:

**۱. الگوی زنجیره‌ای (Sequential):** رایج‌ترین حالت که در آن هر مرحله منتظر مرحله قبل می‌ماند.

Go

```
var result string
err := workflow.ExecuteActivity(ctx, MyActivity, input).Get(ctx, &result)
```

**۲. الگوی موازی (Parallel):** برای کاهش زمان کلی (Latency)، ابتدا همه Activityها را استارت می‌زنیم (بدون گرفتن نتیجه) و سپس نتایج را جمع‌آوری می‌کنیم.

Go

```
// شروع همزمان (Non-blocking)
f1 := workflow.ExecuteActivity(ctx, ActivityA, in1)
f2 := workflow.ExecuteActivity(ctx, ActivityB, in2)

// دریافت نتایج (Blocking)
err1 := f1.Get(ctx, &res1)
err2 := f2.Get(ctx, &res2)
```

> [!TIP] اگر سه Activity هر کدام ۱۰ ثانیه زمان ببرند، در حالت اول ۳۰ ثانیه و در حالت موازی حدوداً ۱۰ ثانیه زمان صرف می‌شود (به شرطی که منابع کافی در Worker موجود باشد).


لینک ها : 


----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
