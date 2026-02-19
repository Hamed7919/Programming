---
created: 2026-02-19
updated: 2026-02-19 11:56
tags:
  - Temporal102
---

# {{Temporal-Backward-Compatibility-Pattern}} 
تغییر در امضای تابع (تعداد، جایگاه یا نوع پارامترها) در Temporal باعث می‌شود که اجراهای فعلی با کد جدید ناسازگار شوند و شکست بخورند.

- **تله**: استفاده از فیلدهای تکی (مانند `name string`, `age int`) در ورودی توابع.
    
- **راهکار**: بسته‌بندی (Encapsulation) تمام پارامترهای ورودی و مقادیر بازگشتی در قالب یک **Struct** واحد.
    
- **مزیت**: شما می‌توانید فیلدهای جدیدی به استراکت اضافه کنید بدون اینکه امضای تابع (Signature) تغییر کند. این کار پایداری سیستم در طول زمان را تضمین می‌کند.
---

# ۵️⃣ چرا Struct مهمه برای Backwards Compatibility؟

بTemporal تاریخچه اجرا (History) رو ذخیره می‌کنه.

اگر امضای تابع عوض بشه:

`func GetGreeting(name string)`

و بعداً تبدیلش کنی به:

`func GetGreeting(name string, age int)`

❌ همه اجراهای قدیمی fail میشن  
چون signature تغییر کرده.

لینک ها : 
[[Temporal-Backward-Compatibility-Pattern]]


----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
