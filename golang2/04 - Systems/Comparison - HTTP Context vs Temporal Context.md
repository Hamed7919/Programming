---
created: 2026-02-19
updated: 2026-02-19 15:25
tags: []
---
# مقایسه کانتکست HTTP و کانتکست Temporal

درک تفاوت بین lifecycleهای زودگذر و توزیع‌شده.

| ویژگی | HTTP Context | Temporal Context |
| :--- | :--- | :--- |
| **طول عمر** | کوتاه (میلی‌ثانیه/ثانیه) | طولانی (دقیقه/سال) |
| **وابستگی** | به TCP Connection و Thread | به Event History و Engine |
| **پایداری** | با کراش سیستم از بین می‌رود | قابل Replay و بازیابی است |
| **مالکیت** | متعلق به Handler/Request | متعلق به Temporal Engine |

## نکته کلیدی مهندسی
در **HTTP**، کانتکست از محیط اجرا (Request) گرفته می‌شود، اما در **Temporal**، کانتکست بخشی از State ماشین اجراست و باید از بیرون به Workflow تزریق شود تا غیرقابل پیش‌بینی (Non-deterministic) نشود.


---

```
func handler(w http.ResponseWriter, r *http.Request) {
    ctx := r.Context()

    dbCall(ctx)
    cacheCall(ctx)
    externalAPI(ctx)
}

```


---
```
func MyWorkflow(ctx workflow.Context, ...)

```



#Temporal #Microservices #DistributedSystems #Golang




----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
