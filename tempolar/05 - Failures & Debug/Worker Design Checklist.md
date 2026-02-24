---
created: 2026-02-24
updated: 2026-02-24 15:45
tags:
  - "#Temporal102"
---
# Worker Production Checklist

✅ بTaskQueue Name ثابت و version-controlled  
✅ بWorkflow و Activity هر دو register شوند  
✅ بGraceful Shutdown با worker.InterruptCh()  
✅ بLogging استاندارد داشته باشد  
✅ بPanic داخل Activity هندل شود

---

Anti-Pattern:

یک Worker برای همه چیز بدون تفکیک Domain

لینک ها : 

[[Hands-On Exercise ـObserving Durable Execution]]


---

## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
