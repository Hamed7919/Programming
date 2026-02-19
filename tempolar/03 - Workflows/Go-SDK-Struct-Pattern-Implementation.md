---
created: 2026-02-19
updated: 2026-02-19 11:59
tags:
  - Temporal102
---

# {{Go-SDK-Struct-Pattern-Implementation}} 
الگوی استاندارد برای تعریف اکتیویتی‌ها و ورک‌فلوهای منعطف در Go:

Go

```
// ۱. تعریف استراکت برای ورودی
type GreetingInput struct {
    Name         string
    LanguageCode string // فیلد جدید که بعداً اضافه شده
}

// ۲. تعریف استراکت برای خروجی
type GreetingOutput struct {
    Greeting string
}

// ۳. استفاده در امضای تابع
func GetTranslatedGreeting(ctx context.Context, input GreetingInput) (GreetingOutput, error) {
    // دسترسی به داده‌ها از طریق استراکت
    msg := fmt.Sprintf("Hello %s", input.Name)
    
    return GreetingOutput{ Greeting: msg }, nil
}
```

- **نکته**: تغییر از `string` به `struct` خودش یک تغییر مخرب (Breaking Change) است؛ پس این کار را باید در **اولین مرحله‌ی توسعه** انجام داد تا مسیر برای تغییرات آینده هموار شود.
---
# ۷️⃣ چرا میگن از اول Struct استفاده کن؟

چون تغییر از:

`func MyWorkflow(ctx workflow.Context, name string)`

به:

`func MyWorkflow(ctx workflow.Context, input MyInput)`

❌ خودش breaking change هست

پس باید از همون روز اول این الگو رو رعایت کنی.

---

لینک ها : 

[[Temporal-IO-Best-Practices]]
[[Temporal-Context-Dualism]]

----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
