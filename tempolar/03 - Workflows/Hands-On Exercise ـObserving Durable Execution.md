---
created: 2026-02-24
updated: 2026-02-24 10:15
tags:
  - Temporal102
---
 فرض می‌کنیم:

> 🔴 روی سیستم تو هیچ پروژه Temporal وجود نداره
> 🔴 هیچ فایلی نداری
> 🔴 از صفر مطلق شروع می‌کنیم

و هدفمون اینه:

> Durable Execution رو خودمون بسازیم و با چشم ببینیم.

کاملاً مفهومی + عملی جلو میریم.

---

# 🎯 چیزی که قراره بسازیم

یک پروژه Go که:

1. یک Workflow دارد
2. یک Activity دارد
3. داخل Workflow یک Timer دارد
4. دو Worker اجرا می‌کنیم
5. یکی را می‌کشیم
6. می‌بینیم اجرای Workflow ادامه پیدا می‌کند

---

# 🧱 مرحله 1 — پیش‌نیازها

باید این‌ها نصب باشند:

### 1️⃣ Go

چک کن:

```bash
go version
```

اگر نیست:

---

### 2️⃣ Temporal CLI

چک کن:

```bash
temporal version
```

اگر نیست:

```bash
brew install temporal
```

یا طبق سایت Temporal نصب کن.

---

# 🚀 مرحله 2 — اجرای Temporal Server

در یک ترمینال:

```bash
temporal server start-dev
```

باید ببینی:

```
Frontend gRPC listening on 127.0.0.1:7233
Web UI available at http://localhost:8233
```

🔵 این ترمینال را نبند.

---

# 📁 مرحله 3 — ساخت پروژه از صفر

در یک ترمینال جدید:

```bash
mkdir durable-demo
cd durable-demo
go mod init durable-demo
```

---

# 📦 مرحله 4 — نصب SDK

```bash
go get go.temporal.io/sdk
```

---
# 🎓 نکته حرفه‌ای Go

در هر پروژه Go، بعد از:

- clone کردن پروژه
    
- یا اضافه کردن dependency جدید
    

تقریباً همیشه باید بزنی:

go mod tidy

این مثل "نفس کشیدن" برای پروژه Go است 😄


---

# 🏗 مرحله 5 — ساخت ساختار پروژه

```bash
mkdir workflows
mkdir activities
mkdir worker
mkdir starter
```

ساختار نهایی:

```
durable-demo/
  go.mod
  workflows/
  activities/
  worker/
  starter/
```

---

# 🧠 مرحله 6 — نوشتن Activity

📁 `activities/activity.go`

```go
package activities

import (
	"context"
	"log"
)

func SampleActivity(ctx context.Context, name string) (string, error) {
	log.Println("Activity started")

	result := "Hello " + name

	log.Println("Activity completed")

	return result, nil
}
```

🔵 این Activity عمداً ساده است.

---

# 🧠 مرحله 7 — نوشتن Workflow (با Timer)

📁 `workflows/workflow.go`

```go
package workflows

import (
	"time"

	"go.temporal.io/sdk/workflow"
)

func DurableWorkflow(ctx workflow.Context, name string) (string, error) {

	logger := workflow.GetLogger(ctx)
	logger.Info("Workflow started")

	ao := workflow.ActivityOptions{
		StartToCloseTimeout: time.Minute,
	}

	ctx = workflow.WithActivityOptions(ctx, ao)

	var result string

	err := workflow.ExecuteActivity(ctx, "SampleActivity", name).Get(ctx, &result)
	if err != nil {
		return "", err
	}

	logger.Info("Activity result received")

	logger.Info("Starting 30-second timer...")

	err = workflow.Sleep(ctx, 30*time.Second)
	if err != nil {
		return "", err
	}

	logger.Info("Timer finished")

	return result + " - Workflow Completed", nil
}
```

🔴 مهم‌ترین خط اینجاست:

```
workflow.Sleep
```

نه `time.Sleep`

---

# ⚙️ مرحله 8 — ساخت Worker

📁 `worker/main.go`

```go
package main

import (
	"log"

	"go.temporal.io/sdk/client"
	"go.temporal.io/sdk/worker"

	"durable-demo/workflows"
	"durable-demo/activities"
)

func main() {

	c, err := client.Dial(client.Options{})
	if err != nil {
		log.Fatalln("Unable to create client", err)
	}
	defer c.Close()

	w := worker.New(c, "durable-task-queue", worker.Options{})

	w.RegisterWorkflow(workflows.DurableWorkflow)
	w.RegisterActivity(activities.SampleActivity)

	err = w.Run(worker.InterruptCh())
	if err != nil {
		log.Fatalln("Unable to start worker", err)
	}
}
```

---

# 🚀 مرحله 9 — ساخت Starter

📁 `starter/main.go`

```go
package main

import (
	"context"
	"log"

	"go.temporal.io/sdk/client"
)

func main() {

	c, err := client.Dial(client.Options{})
	if err != nil {
		log.Fatalln("Unable to create client", err)
	}
	defer c.Close()

	we, err := c.ExecuteWorkflow(
		context.Background(),
		client.StartWorkflowOptions{
			ID:        "durable-workflow-1",
			TaskQueue: "durable-task-queue",
		},
		"DurableWorkflow",
		"Hamed",
	)

	if err != nil {
		log.Fatalln("Unable to execute workflow", err)
	}

	log.Println("Started workflow", we.GetID())
}
```

---

# 🏃‍♂️ مرحله 10 — اجرا

## 1️⃣ اجرای Worker اول

```bash
go run worker/main.go
```

---

## 2️⃣ اجرای Worker دوم (در ترمینال دیگر)

```bash
go run worker/main.go
```

---

## 3️⃣ اجرای Workflow

```bash
go run starter/main.go
```

---

# 💣 مرحله 11 — تست Durable Execution

وقتی دیدی:

```
Starting 30-second timer...
```

یکی از Worker ها را بکش:

```
Ctrl + C
```

30 ثانیه صبر کن.

خواهی دید:

Worker دوم ادامه اجرا را انجام می‌دهد و:

```
Timer finished
```

چاپ می‌شود.

---

# 🔎 مرحله 12 — بررسی در UI

برو به:

```
http://localhost:8233
```

Workflow را باز کن.

Event History را ببین.

باید ببینی:

```
ActivityScheduled
ActivityCompleted
TimerStarted
TimerFired
WorkflowCompleted
```

---

# 🧠 حالا مهم‌ترین سوال

اگر هر دو Worker را قبل از اتمام Timer بکشی چی می‌شود؟

پاسخ:

هیچ چیز از بین نمی‌رود.

وقتی دوباره Worker را اجرا کنی،
Workflow ادامه پیدا می‌کند.

---

# 🔥 این همان Durable Execution است

Worker فقط executor است.
State در Temporal Server ذخیره می‌شود.

---

لینک ها : 


----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
