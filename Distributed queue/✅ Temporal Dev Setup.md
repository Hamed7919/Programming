عالیه—پس بریم سراغ **چک‌لیست حرفه‌ای Temporal Dev Setup** که مثل یک **Runbook مهندسی** همیشه بهت می‌گه کجای سیستم مشکل دارد و چرا.  
این چیزی‌ست که تیم‌های Backend واقعی استفاده می‌کنند 👇

---

# ✅ Temporal Dev Setup – چک‌لیست طلایی مهندسی

این ترتیب **بسیار مهم است** (مثل Debugging OSI Layer):

---

# 🧩 مرحله 1: Go Project سالم است؟

## 🔹 چک سریع

```bash
go mod tidy
```

## 🔹 باید این دو فایل درست باشند

```
go.mod
go.sum
```

اگر go.sum نبود یا ناقص بود → کل dependency سیستم unstable است.

---

# 🧩 مرحله 2: Temporal Server روشن است؟

## 🔹 دستور استاندارد dev

```bash
temporal server start-dev
```

باید ببینی:

```
Server:  localhost:7233
UI:      http://localhost:8233
```

---

# 🧩 مرحله 3: پورت واقعاً باز است؟ (OS level)

```bash
lsof -i :7233
```

یا:

```bash
nc -vz 127.0.0.1 7233
```

✅ باید succeeded بدهد.

---

# 🧩 مرحله 4: CLI می‌تواند به Cluster وصل شود؟

```bash
temporal operator cluster health --address 127.0.0.1:7233
```

اگر:

```
SERVING
```

→ Network + gRPC OK است.

---

# 🧩 مرحله 5: Namespace وجود دارد؟

```bash
temporal namespace list --address 127.0.0.1:7233
```

باید ببینی:

```
default
```

اگر نبود:

```bash
temporal namespace register default
```

---

# 🧩 مرحله 6: Worker وصل است؟

Worker باید همیشه در یک ترمینال جدا اجرا شود:

```bash
go run worker/main.go
```

باید log شبیه این بدهد:

```
Started Worker
```

---

# 🧩 مرحله 7: Workflow را درست اجرا کن

```bash
temporal workflow start \
  --address 127.0.0.1:7233 \
  --namespace default \
  --type GreetingWorkflow \
  --task-queue greeting-tasks \
  --workflow-id test-1 \
  --input '"Hamed"'
```

---

# 🧠 Debug سریع با Temporal UI

برو در مرورگر:

```
http://localhost:8233
```

و ببین:

- Workflow اجرا شده؟
    
- Failed یا Running؟
    
- Task Queue خالی است؟
    

---

# 🧨 90٪ مشکلات Temporal در Dev

## ❌ localhost vs 127.0.0.1

## ❌ Namespace اشتباه

## ❌ Worker اجرا نشده

## ❌ Task Queue mismatch

## ❌ Firewall یا VPN

---

# 🧠 Senior Engineer Mental Model

Temporal سه چیز دارد:

```
Server (brain)
Worker (muscle)
CLI (remote control)
```

اگر یکی قطع شود → سیستم مرده است.

---

# 🧪 Super Debug Command (خیلی حرفه‌ای)

```bash
temporal workflow list --address 127.0.0.1:7233
```

اگر این کار کرد → همه چیز سالم است.

---

# 🧠 Bonus: Dev Setup Script (اتوماسیون)

این را ذخیره کن: `check_temporal.sh`

```bash
#!/bin/bash

echo "== Go deps =="
go mod tidy

echo "== Temporal health =="
temporal operator cluster health --address 127.0.0.1:7233

echo "== Namespaces =="
temporal namespace list --address 127.0.0.1:7233

echo "== Workflows =="
temporal workflow list --address 127.0.0.1:7233
```

---

# 🚀 اگر بخواهی Level Up خیلی حرفه‌ای

می‌تونم بهت یاد بدم:

- Dockerized Temporal dev environment (مثل Production)
    
- Makefile برای start همه چیز با یک دستور
    
- Auto-reload worker مثل nodemon
    
- Debugging workflow history مثل distributed tracing
    
- Best practices ساخت task-queue architecture
    
- Temporal vs Kafka vs RabbitMQ ذهنیت معماری
    

---

# ❓ یک سؤال خیلی مهم برای مسیر حرفه‌ای تو

تو Temporal را برای چه چیزی می‌خواهی؟  
(learning, startup idea, backend job, distributed systems research؟)

اگر هدف را بگویی، من دقیقاً یک roadmap مهندسی‌شده بهت می‌دهم که در چند هفته به **Senior-level Temporal + Go** برسی.