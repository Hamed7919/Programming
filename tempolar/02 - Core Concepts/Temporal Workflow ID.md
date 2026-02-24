---
created: 2026-02-20
updated: 2026-02-20 09:09
tags:
  - Temporal102
source:
---
در Temporal، هر Workflow Execution نیاز به یک شناسه منحصر‌به‌فرد به نام **Workflow ID** دارد. برخلاف IDهای سیستمی، این ID باید دارای معنای تجاری (Business Logic) باشد.

**مثال‌ها:**

- سیستم سفارشات: `order-number-90743812`
    
- سیستم بانکی: `loan-account-28430614`
---
# 🧠 یه تفکیک خیلی مهم

دو تا چیز رو باید جدا کنیم:

|مفهوم|معنی|
|---|---|
|Workflow Definition|کدی که نوشتی|
|Workflow Execution|یک اجرای مشخص با یک ID|

تو هر چقدر بخوای می‌تونی از یک Definition اجرا بسازی.

---

# 🎯 مثال تو: محاسبه اکسیژن ماشین

فرض کن یه Workflow داری:

func OxygenCalculationWorkflow(ctx workflow.Context, input OxygenInput) error

این فقط یه "تعریف"ه.

حالا اگر:

- ساعت 10:00 یک محاسبه انجام شد
    
- ساعت 10:01 دوباره محاسبه لازم شد
    

تو باید یه **Execution جدید** شروع کنی.

---

# ❗ اشتباه رایج

بعضی‌ها فکر می‌کنن:

Workflow ID = نوع Workflow

نه ❌

Workflow ID = شناسه یک اجرای خاص
## لینک‌ها


[[Workflow ID Uniqueness]]
[[Setting Workflow ID in Go]]
[[Workflow ID Conflict Handling]]
[[Workflow ID Reuse Policy]]



---

## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود


