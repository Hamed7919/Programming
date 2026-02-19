---
created: 2026-02-19
updated: 2026-02-19 12:36
tags:
  - Temporal102
---

# {{Workflow-Context-Constraints}} 
در داخل ورک‌فلو، هرگز نباید از پکیج‌های استاندارد که به `context.Context` نیاز دارند استفاده کرد. به جای آن‌ها از معادل‌های تمپورالی استفاده کنید:

- ❌ `time.Sleep(d)` $\rightarrow$ ✅ `workflow.Sleep(ctx, d)`
    
- ❌ `select { case <-time.After(d): }` $\rightarrow$ ✅ `workflow.NewTimer(ctx, d)`
    
- ❌ `go func() { ... }()` $\rightarrow$ ✅ `workflow.Go(ctx, func() { ... })`
    
- ❌ `context.Background()` $\rightarrow$ ✅ (همیشه از `ctx` ورودی ورک‌فلو استفاده کنید)- 


لینک ها : 


----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
