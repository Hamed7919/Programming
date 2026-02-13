عالی 👌  
الان می‌خوای همین سناریو رو بر اساس **Temporal 101 with Go** بازطراحی کنیم — نه Node — و ایرادهای مفهومی و فنی‌ش رو هم اصلاح کنیم.

من این رو دقیقاً مطابق Go SDK و تجربه‌ای که همین الان با خطاهاش داشتی (context deadline, workflow.Context, HostPort و…) می‌چینم تا تمیز و بدون ابهام باشه.

---

# 🧪 Exercise 1 – Hello Workflow (Go Version)

> فرض: فقط Temporal نصب شده و هیچی بلد نیستیم.

---

# 🧠 قبل از شروع — معماری رو بفهم

Temporal سه جزء اصلی داره:

```
[ Temporal Server ]
        ↑
     (poll)
[ Worker ]  ← گوش می‌دهد روی Task Queue
        ↑
[ Workflow Execution ] ← توسط CLI یا کد استارت می‌شود
```

اگر Worker نباشد → Workflow اجرا نمی‌شود  
اگر Server نباشد → هیچ‌چیز وجود ندارد

---

# ✅ Step 1 — چک نصب

```bash
temporal version
go version
```

---

# ✅ Step 2 — اجرای Server (Terminal #1)

```bash
temporal server start-dev
```

باید ببینی:

```
Server:  localhost:7233
UI:      http://localhost:8233
```

این ترمینال را نبند ❗

---

# ✅ Step 3 — ساخت پروژه Go تمیز (خیلی مهم)

در یک ترمینال جدید (Terminal #2):

```bash
mkdir temporal-hello
cd temporal-hello
go mod init temporal-hello
```

نصب SDK:

```bash
go get go.temporal.io/sdk
```

ساختار پروژه:

```
temporal-hello/
│
├── go.mod
├── worker/
│     └── main.go
└── workflows/
      └── greeting.go
```

---

# ✅ Step 4 — نوشتن Workflow (درست و بدون خطا)

📁 `workflows/greeting.go`

```go
package workflows

import (
	"go.temporal.io/sdk/workflow"
)

func GreetingWorkflow(ctx workflow.Context, name string) (string, error) {
	return "Hello, " + name + "!", nil
}
```

🔴 نکته مهم:

- حتماً `workflow.Context`
    
- نه `context.Context`
    

این همون اروری بود که قبلاً گرفتی.

---

# ✅ Step 5 — نوشتن Worker (نسخه صحیح با HostPort)

📁 `worker/main.go`

```go
package main

import (
	"log"

	"go.temporal.io/sdk/client"
	"go.temporal.io/sdk/worker"

	"temporal-hello/workflows"
)

func main() {

	// اتصال صریح به سرور dev
	c, err := client.Dial(client.Options{
		HostPort: "localhost:7233",
	})
	if err != nil {
		log.Fatalln("connection error:", err)
	}
	defer c.Close()

	// Worker روی Task Queue
	w := worker.New(c, "greeting-tasks", worker.Options{})

	// ثبت Workflow
	w.RegisterWorkflow(workflows.GreetingWorkflow)

	log.Println("✅ Worker started...")

	// شروع polling
	err = w.Run(worker.InterruptCh())
	if err != nil {
		log.Fatalln("worker error:", err)
	}
}
```

🔴 اینجا مهم‌ترین اصلاح نسبت به تمرین قبلی:

```
HostPort: "localhost:7233"
```

اگر این نباشد → context deadline exceeded

---

# ✅ Step 6 — اجرای Worker

در Terminal #2:

```bash
go run worker/main.go
```

باید ببینی:

```
✅ Worker started...
```

این ترمینال را باز نگه دار.

---

# ✅ Step 7 — اجرای Workflow (Terminal #3)

```bash
temporal workflow start \
  --address localhost:7233 \
  --type GreetingWorkflow \
  --task-queue greeting-tasks \
  --workflow-id hello-1 \
  --input '"Hamed"'
```

اگر درست باشد:

```
Started workflow
```

---

# ✅ Step 8 — دیدن نتیجه

مرورگر:

```
http://localhost:8233
```

باید ببینی:

- Workflow ID: hello-1
    
- Status: Completed
    
- Result: Hello, Hamed!
    

---

# 🔍 اصلاح ایرادهای نسخه قبلی تمرینت

نسخه قبلی چند مشکل داشت:

### ❌ 1. ترکیب Node و Go

تمرین Node-style Worker داشت ولی تو Go کار می‌کنی.

### ❌ 2. اشاره نکردن به HostPort

در Go اگر صریحاً HostPort ندهی → timeout می‌گیری (که گرفتی).

### ❌ 3. توضیح ندادن workflow.Context

این باعث panic شد که دیدیم.

### ❌ 4. نگفتن CLI ممکنه نیاز به --address داشته باشه

نسخه‌های مختلف CLI این رو لازم دارن.

---

# 🧠 الان واقعاً چی یاد گرفتی؟

- Server = مغز سیستم
    
- Worker = مصرف‌کننده Task Queue
    
- Workflow = تعریف منطق
    
- CLI = شروع Execution
    
- UI = مشاهده
    

و مهم‌تر از همه:

> Worker بدون Task Queue درست = بی‌معنی  
> Workflow بدون Worker = Pending می‌ماند  
> HostPort اشتباه = timeout

---

# 🚀 مرحله بعدی پیشنهادی (برای عمیق‌تر فهمیدن)

اگر واقعاً می‌خوای Temporal رو بفهمی، این‌ها رو تست کن:

1️⃣ عمداً اسم task queue رو اشتباه بزن  
2️⃣ Worker رو خاموش کن و Workflow اجرا کن  
3️⃣ از Go Client به جای CLI استارت کن  
4️⃣ Activity اضافه کنیم  
5️⃣ Retry و Timeout رو ببینیم

---

حالا سوال مهم 👇  
می‌خوای:

- 🔹 اجرای Workflow از داخل Go (بدون CLI)
    
- 🔹 یا اضافه کردن Activity
    
- 🔹 یا بررسی دقیق Task Queue behavior
    

کدوم رو عمیق بریم؟