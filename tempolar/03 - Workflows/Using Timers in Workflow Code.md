---
created: 2026-02-20
updated: 2026-02-20 11:19
tags:
  - Temporal102
---
باشه 👌 خیلی ساده، خلاصه و تمیز:

---

# 📝 استفاده از Timer در Workflow (Temporal)

در Workflow نباید از `time.Sleep` معمولی Go استفاده کنی ❌  
چون Workflow باید deterministic باشه.

به جاش از ابزارهای خود Temporal استفاده می‌کنیم ✅

---

## ⏳ 1️⃣ ساده‌ترین روش: `workflow.Sleep`

```go
err := workflow.Sleep(ctx, 10*time.Second)
```

### یعنی چی؟

یعنی:

> اجرای Workflow به مدت ۱۰ ثانیه متوقف می‌شود.

---

### چرا باید از این استفاده کنیم؟

- امن برای Replay ✅
    
- جایگزین درست `time.Sleep` ❌
    
- قابل Cancel شدن توسط Temporal ✅
    
- state در دیتابیس ذخیره می‌شود ✅
    

---

### چه زمانی استفاده کنیم؟

وقتی فقط می‌خواهی Workflow:

- کمی صبر کند
    
- بعد ادامه دهد
    

مثلاً:

- صبر قبل از Retry
    
- Delay بین دو مرحله
    

---

## 🔥 خیلی مهم

❌ این کار را نکن:

```go
time.Sleep(10 * time.Second)
```

چون Replay را خراب می‌کند.

---

## 🎯 خلاصه یک خطی

در Workflow همیشه از:

```go
workflow.Sleep
```

استفاده کن، نه `time.Sleep`.

---

عالی 👌 بریم خیلی ساده و قابل فهم توضیحش بدیم:

---

# ⏳ 2️⃣ `workflow.NewTimer`

این روش برای وقتی‌ست که کنترل بیشتری می‌خواهی.

```go
timerFuture := workflow.NewTimer(ctx, 5*time.Second)
timerFuture.Get(ctx, nil)
```

---

# 🧠 فرقش با `workflow.Sleep` چیه؟

### `workflow.Sleep`

همون لحظه اجرای Workflow رو متوقف می‌کنه.

---

### `workflow.NewTimer`

فقط تایمر رو **می‌سازه**  
ولی اجرای Workflow همون لحظه متوقف نمی‌شه.

بهت یه **Future** می‌ده.

---

# 📦 Future یعنی چی ساده؟

Future یعنی:

> نتیجه‌ای که بعداً آماده می‌شود.

یعنی تایمر شروع شده،  
اما تو هر وقت خواستی می‌تونی منتظرش بمونی.

---

# 🎯 مثال ساده ذهنی

فرض کن می‌خوای:

- ۵ ثانیه صبر کنی
    
- ولی همزمان یه Activity هم اجرا کنی
    

با `Sleep` نمی‌تونی.

اما با `NewTimer` می‌تونی.

---

# 🧪 مثال کاربردی

```go
timer := workflow.NewTimer(ctx, 5*time.Second)

activityFuture := workflow.ExecuteActivity(ctx, MyActivity)

selector := workflow.NewSelector(ctx)

selector.AddFuture(timer, func(f workflow.Future) {
    fmt.Println("Timer finished")
})

selector.AddFuture(activityFuture, func(f workflow.Future) {
    fmt.Println("Activity finished")
})

selector.Select(ctx)
```

اینجا هر کدوم زودتر تموم بشه اجرا میشه.

---

# 🧠 چه زمانی از NewTimer استفاده کنیم؟

وقتی می‌خواهی:

- تایمر همزمان با کار دیگر اجرا شود
    
- بین چند چیز select بزنی
    
- race condition طراحی کنی
    
- timeout سفارشی بسازی
    

---

# 🎯 خلاصه خیلی ساده

| اگر فقط می‌خواهی صبر کنی | `workflow.Sleep` |  
| اگر می‌خواهی همزمان کار دیگر هم انجام شود | `workflow.NewTimer` |

---

# 💡 یک جمله طلایی

`Sleep` = صبر ساده  
`NewTimer` = تایمر حرفه‌ای با کنترل بیشتر

---

اگر بخواهی، می‌توانم یک مثال واقعی Timeout برای Activity با NewTimer برات بکشم که خیلی عمیق جا بیفته 👌


---



```markdown
# Timer as a Distributed Systems Primitive

در Temporal، Timer فقط Delay نیست.

بلکه یک Primitive سیستمی است که:

- Fail-safe است
- Cluster-managed است
- Replay-safe است
- Deterministic است

---

## رفتار هنگام Crash شدن Worker

سناریو:

Timer = 10 ثانیه  
Worker بعد از 3 ثانیه Crash می‌کند

حالت 1:
Worker بعد از 2 ثانیه Restart شود
→ 5 ثانیه باقی‌مانده صبر می‌کند
→ Execution طبیعی ادامه می‌یابد

حالت 2:
Worker بعد از 20 دقیقه Restart شود
→ Timer قبلاً Fire شده
→ Execution بلافاصله ادامه می‌یابد

نتیجه:
Timer به Worker وابسته نیست.
