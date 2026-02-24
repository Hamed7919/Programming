---
created: 2026-02-24
updated: 2026-02-24 09:57
tags: []
---
عالی 👌  
الان برات یک **سیستم ECU شبیه‌سازی‌شده کامل** می‌نویسم که:

- ✅ معماری Parent → Child درست دارد
    
- ✅ سه حالت توقف دارد (Manual Cancel / Timeout / Deadline)
    
- ✅ defer cancel رعایت شده
    
- ✅ context اولین پارامتر است
    
- ✅ تشخیص نوع خطا انجام می‌شود
    
- ✅ هیچ Background اضافی ساخته نشده
    
- ✅ cancellation به‌صورت cooperative انجام می‌شود
    

---

# 🧠 سناریو مهندسی

ما این ساختار را پیاده‌سازی می‌کنیم:

```
Root (Background)
   │
   └── engineCtx (Manual Cancel)
        │
        └── cycleCtx (Timeout 3ms)
              │
              └── injectorCtx (Deadline سخت‌تر)
```

سه حالت توقف:

1. راننده موتور را خاموش می‌کند → Manual Cancel
    
2. کل سیکل بیش از حد طول می‌کشد → Timeout
    
3. انژکتور تا زمان مشخص فرمان نگیرد → Deadline
    

---

# ✅ کد کامل سیستم ECU

```go
package main

import (
	"context"
	"errors"
	"fmt"
	"time"
)

func main() {

	// =========================
	// ROOT (فقط یک بار در کل برنامه)
	// =========================
	root := context.Background()

	// =========================
	// ENGINE CONTEXT (Manual Cancel)
	// =========================
	engineCtx, stopEngine := context.WithCancel(root)
	defer stopEngine() // ایمنی نهایی

	// شبیه‌سازی خاموش شدن موتور بعد از 5ms
	go func() {
		time.Sleep(5 * time.Millisecond)
		fmt.Println(">>> Driver turned engine OFF (Manual Cancel)")
		stopEngine()
	}()

	// اجرای چند سیکل احتراق
	for i := 1; i <= 3; i++ {

		fmt.Println("\n--- Combustion Cycle", i, "---")

		err := runCycle(engineCtx)

		if err != nil {
			handleError(err)
			break
		}
	}
}
```

---

# 🔥 اجرای یک سیکل احتراق

```go
func runCycle(parent context.Context) error {

	// =========================
	// TIMEOUT برای کل سیکل
	// =========================
	cycleCtx, cancel := context.WithTimeout(parent, 3*time.Millisecond)
	defer cancel()

	air, err := readAir(cycleCtx)
	if err != nil {
		return err
	}

	fuel, err := calculateFuel(cycleCtx, air)
	if err != nil {
		return err
	}

	// =========================
	// DEADLINE سخت‌تر برای انژکتور
	// =========================
	deadline := time.Now().Add(1 * time.Millisecond)
	injectorCtx, cancelInjector := context.WithDeadline(cycleCtx, deadline)
	defer cancelInjector()

	err = injectFuel(injectorCtx, fuel)
	if err != nil {
		return err
	}

	fmt.Println("✅ Cycle completed successfully")
	return nil
}
```

---

# 🌬 خواندن سنسور هوا

```go
func readAir(ctx context.Context) (float64, error) {

	select {
	case <-ctx.Done():
		return 0, ctx.Err()
	case <-time.After(1 * time.Millisecond):
		fmt.Println("Air sensor read")
		return 10.0, nil
	}
}
```

---

# 🧮 محاسبه سوخت

```go
func calculateFuel(ctx context.Context, air float64) (float64, error) {

	select {
	case <-ctx.Done():
		return 0, ctx.Err()
	default:
		time.Sleep(1 * time.Millisecond)
		fmt.Println("Fuel calculated")
		return air * 0.8, nil
	}
}
```

---

# 💉 ارسال به انژکتور

```go
func injectFuel(ctx context.Context, fuel float64) error {

	select {
	case <-ctx.Done():
		return ctx.Err()
	case <-time.After(2 * time.Millisecond): // عمداً کندتر از deadline
		fmt.Println("Fuel injected:", fuel)
		return nil
	}
}
```

---

# 🛑 مدیریت نوع خطا

```go
func handleError(err error) {

	if errors.Is(err, context.Canceled) {
		fmt.Println("🛑 Engine stopped manually")
		return
	}

	if errors.Is(err, context.DeadlineExceeded) {
		fmt.Println("⏱ Deadline or Timeout exceeded")
		return
	}

	fmt.Println("⚠ Unexpected error:", err)
}
```

---

# 🧠 حالا دقیق ببین چه اتفاقی می‌افتد

### حالت 1 — Deadline انژکتور

چون injectFuel عمداً 2ms طول می‌کشد  
ولی deadline فقط 1ms است

پس:

```
context deadline exceeded
```

فقط injectorCtx متوقف می‌شود  
cycleCtx زنده است  
engineCtx زنده است

---

### حالت 2 — Timeout کل سیکل

اگر کل عملیات بیش از 3ms شود:

```
DeadlineExceeded
```

cycleCtx متوقف می‌شود  
injector هم چون child است متوقف می‌شود  
engineCtx زنده می‌ماند

---

### حالت 3 — Manual Cancel

وقتی stopEngine() صدا زده شود:

```
context canceled
```

کل درخت context متوقف می‌شود.

---

# 💎 چرا این کد مهندسی است؟

✔ root فقط یک بار ساخته شده  
✔ context اولین پارامتر است  
✔ defer cancel رعایت شده  
✔ child فقط وقتی نیاز بوده ساخته شده  
✔ تشخیص نوع خطا انجام شده  
✔ cooperative cancellation رعایت شده

---

# 🧠 تصویر ذهنی نهایی که باید داشته باشی

- engineCtx = عمر موتور
    
- cycleCtx = عمر هر احتراق
    
- injectorCtx = حساس‌ترین عملیات
    
- cancel همیشه برای آزادسازی منابع
    
- context فقط سیگنال می‌دهد
    
- تو باید آن را چک کنی
    

---

اگر بخواهی، قدم بعدی را می‌توانیم برویم سمت:

- 🚀 همین معماری در یک REST API واقعی
    
- 🧵 ترکیب context با worker pool
    
- 🏗 پیاده‌سازی همین سیستم با الگوی Service / Logic / Adapter
    

کدام را عمیق‌تر بررسی کنیم؟



----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
