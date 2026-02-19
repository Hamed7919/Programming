---
created: 2026-02-19
updated: 2026-02-19 15:35
tags:
  - Temporal102
---
برای جلوگیری از خطاهای تایپی (Typo)، همیشه نام صف را در یک ثابت (Constant) تعریف کنید که هم توسط کلاینت و هم توسط ورکر ارجاع داده شود.

Go

```
package app

// تعریف متمرکز نام صف
const GreetingTaskQueue = "GREETING_TASK_QUEUE"
```

- **در کلاینت (Start Workflow):**
    
    Go
    
    ```
    TaskQueue: app.GreetingTaskQueue,
    ```
    
- **در ورکر (Worker Configuration):**
    
    Go
    
    ```
    w := worker.New(c, app.GreetingTaskQueue, worker.Options{})
    ```
    

> **نکته برای سیستم‌های Polyglot:** اگر کلاینت شما با زبان دیگری (مثلاً Python) نوشته شده، امکان استفاده از Constant مشترک وجود ندارد. در این صورت، استفاده از فایل‌های کانفیگ (YAML/JSON) یا متغیرهای محیطی (Environment Variables) پیشنهاد می‌شود.
- 


لینک ها : 


----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
