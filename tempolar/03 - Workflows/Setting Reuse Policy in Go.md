---
created: 2026-02-20
updated: 2026-02-20 09:20
tags:
  - Temporal102
---
برای استفاده از این سیاست‌ها در Go، باید پکیج `enums` را وارد کنید:

Go

```
import (
    enums "go.temporal.io/api/enums/v1"
)

options := client.StartWorkflowOptions{
    ID:                    "order-123",
    TaskQueue:             "TRANSFER_MONEY_TASK_QUEUE",
    // تعیین سیاست استفاده مجدد
    WorkflowIDReusePolicy: enums.WORKFLOW_ID_REUSE_POLICY_ALLOW_DUPLICATE_FAILED_ONLY,
}
```

> [!TIP] نکته کاربردی سیاست `TERMINATE_IF_RUNNING` برای سناریوهایی مثل "فقط آخرین درخواست کاربر معتبر است" (مثل تغییر آدرس در حال پردازش) بسیار مفید است.

لینک ها : 


----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
