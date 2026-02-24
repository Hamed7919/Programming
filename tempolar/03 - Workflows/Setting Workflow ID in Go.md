---
created: 2026-02-20
updated: 2026-02-20 09:12
tags:
  - Temporal102
---
برای تعیین Workflow ID در Go، از ساختار `StartWorkflowOptions` استفاده می‌کنیم:

Go

```
options := client.StartWorkflowOptions{
    ID:        "process-order-number-" + input.OrderNumber,
    TaskQueue: app.TaskQueueName,
}

run, err := c.ExecuteWorkflow(ctx, options, ProcessOrderWorkflow, input)
```

**نکته:** همیشه سعی کنید ID را ترکیبی از نوع تسک و یک شناسه منحصربه‌فرد (مانند OrderNumber) قرار دهید.


لینک ها : 


----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
