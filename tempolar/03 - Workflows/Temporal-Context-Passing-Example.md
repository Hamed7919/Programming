---
created: 2026-02-19
updated: 2026-02-19 12:37
tags:
  - Temporal102
---

```
# 
// در ورک‌فلو (دنیای انتزاعی)
`func MyWorkflow(ctx workflow.Context) error {`
    `// استفاده از ctx برای اجرای اکتیویتی`
    `return workflow.ExecuteActivity(ctx, MyActivity).Get(ctx, nil)`
`}`

`// در اکتیویتی (دنیای واقعی)`
`func MyActivity(ctx context.Context) error {`
    `// استفاده از ctx واقعی برای عملیات شبکه`
    `req, _ := http.NewRequestWithContext(ctx, "GET", "http://api.com", nil)`
    `return http.DefaultClient.Do(req)`
`}`


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
