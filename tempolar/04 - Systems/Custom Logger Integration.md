---
created: 2026-02-20
updated: 2026-02-20 09:45
tags:
  - Temporal102
---
عالی 👌  
بریم خیلی ساده، هم مفهومی هم سینتکس.

---

# 🧠 اول مفهوم ساده

در Temporal Technologies وقتی Worker یا Client اجرا میشه، خودش لاگ تولید می‌کنه.

اما:

- لاگر پیش‌فرض ساده‌ست
    
- فقط stdout
    
- level بندی حرفه‌ای نداره
    
- JSON structured logging نداره
    

در Production ما معمولاً از یه لاگر حرفه‌ای مثل  
Uber Technologies’s Zap استفاده می‌کنیم.

---

# 🎯 هدف چیه؟

می‌خوایم بگیم:

> Temporal به جای لاگر خودش، از لاگر ما استفاده کنه.

---

# 🧩 چرا نیاز به Adapter داریم؟

Temporal انتظار داره یه چیزی از نوع:

```go
log.Logger
```

بگیره.

اما Zap یه نوع دیگه داره:

```go
*zap.Logger
```

پس مستقیم نمی‌تونیم بدیمش.

باید یه Adapter بنویسیم که:

Temporal Logger Interface  
→ تبدیلش کنه به  
Zap Logger

---

# 🏗 تصویر ذهنی ساده

```
Temporal ----> (log.Logger interface)
                    ↓
                Adapter
                    ↓
                 Zap
```

---

# 🧠 قدم ۱: ساخت Zap Logger

```go
import "go.uber.org/zap"

zapLogger, _ := zap.NewProduction()
```

---

# 🧠 قدم ۲: ساخت Adapter

باید یه struct بسازیم که interface مربوط به Temporal رو implement کنه.

Temporal این interface رو می‌خواد:

```go
type Logger interface {
    Debug(msg string, keyvals ...interface{})
    Info(msg string, keyvals ...interface{})
    Warn(msg string, keyvals ...interface{})
    Error(msg string, keyvals ...interface{})
}
```

---

## پیاده‌سازی ساده Adapter

```go
type ZapAdapter struct {
    logger *zap.Logger
}

func NewZapAdapter(l *zap.Logger) *ZapAdapter {
    return &ZapAdapter{logger: l}
}

func (z *ZapAdapter) Debug(msg string, keyvals ...interface{}) {
    z.logger.Sugar().Debugw(msg, keyvals...)
}

func (z *ZapAdapter) Info(msg string, keyvals ...interface{}) {
    z.logger.Sugar().Infow(msg, keyvals...)
}

func (z *ZapAdapter) Warn(msg string, keyvals ...interface{}) {
    z.logger.Sugar().Warnw(msg, keyvals...)
}

func (z *ZapAdapter) Error(msg string, keyvals ...interface{}) {
    z.logger.Sugar().Errorw(msg, keyvals...)
}
```

---

# 🧠 قدم ۳: تزریق به Client

```go
customLogger := NewZapAdapter(zapLogger)

c, err := client.Dial(client.Options{
    HostPort: "localhost:7233",
    Logger:   customLogger,
})
```

از این لحظه:

- Client
    
- Worker
    
- Activity logs
    
- Workflow system logs
    

همه از Zap استفاده می‌کنن.

---

# 🔥 نکته مهم Production

چرا این مهمه؟

چون در Production معمولاً:

- لاگ‌ها باید JSON باشن
    
- باید قابل ingest در ELK باشن
    
- باید level filtering داشته باشن
    
- باید correlation ID داشته باشن
    

Zap اینا رو میده.

---

# 🧠 فرق default و Zap

|ویژگی|Default|Zap|
|---|---|---|
|JSON|❌|✅|
|High performance|معمولی|خیلی سریع|
|Structured logging|❌|✅|
|Log levels|محدود|کامل|

---

# 🎯 خلاصه خیلی ساده

Temporal میگه:

> من یه Logger می‌خوام که اینترفیس منو پیاده کنه.

Zap میگه:

> من Logger خودمو دارم.

تو میگی:

> باشه، من بینتون پل می‌زنم 😄

اون پل همون Adapter هست.

---

اگر بخوای می‌تونم مرحله بعدی رو باز کنم:

- چطوری correlation id هر workflow رو داخل Zap inject کنیم
    
- یا چطوری log level رو per environment کنترل کنیم
    
- یا چطوری لاگ workflow و activity رو جدا کنیم
    

کدومو می‌خوای عمیق‌تر بررسی کنیم؟ 🚀
## لینک‌ها




----
## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [x] #to-test      ← باید در عمل تست شود
- [x] #to-review    ← نیاز به بازبینی و اصلاح
- [ ] #ready        ← آماده استفاده و استناد
