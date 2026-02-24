# نقشه محتوای همزمانی و کانتکست در Go

این MOC نقطه شروع یادگیری مدیریت Lifecycle و Concurrency در زبان Go است.

## مفاهیم پایه (Core Concepts)

- [[Philosophy of Go Context]] - ماهیت و چرایی.
- [[Go Context Done Method]] - درک سیگنال توقف.
- [[Context Triggers - Cancel vs Timeout vs Deadline]] - چه زمانی کانتکست بسته می‌شود.

## الگوهای اجرایی (Workflows)
- [[Graceful Shutdown Pattern with Select]] - الگوی استاندارد استفاده از Done.
- 
- [[Go Context Best Practices]] - بایدها و نبایدها.

## معماری سیستم (Systems)
- [[Context Propagation and Hierarchy]] - ساختار درختی و وراثت کانتکست.
- [[Comparison - HTTP Context vs Temporal Context]] - تفاوت کانتکست‌های زودگذر و ماندگار.
- - [[ECU Case Study - Context in Practice]] - مثال واقعی سیستم انژکتور.

## عیب‌یابی و خطاها (Failures & Debug)
- [[Context Error Handling]] - تشخیص نوع خطا.

#Golang #Concurrency #Context


---
