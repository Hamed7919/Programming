---
created: 2026-02-20
updated: 2026-02-20 11:17
tags:
  - Temporal102
source:
---
# Durable Timers

## تعریف

بTimer در Temporal مکانیزمی برای ایجاد Delay داخل Workflow Execution است.

ویژگی مهم:
بTimers توسط Temporal Cluster نگهداری می‌شوند (نه Worker).

بنابراین:
- بWorker هنگام انتظار برای Timer منابع مصرف نمی‌کند
- بTimer در برابر Crash شدن Worker مقاوم است (Durable)

---

## ویژگی‌های اصلی

- حداقل مدت: 1 ثانیه (کمتر از آن توصیه نمی‌شود)
- حداکثر مدت: از چند ثانیه تا چند سال
- دقت زیر 1 ثانیه قابل اعتماد نیست (به دلیل latency بین Worker و Cluster)

---

## چرا Timer در Temporal خاص است؟

در سیستم‌های معمولی:
اگر process بخوابد و crash کند → Timer از بین می‌رود ❌

در Temporal:
بTimer داخل Cluster ثبت می‌شود → مستقل از Worker اجرا می‌شود ✅

این یعنی:
ب Timer بخشی از state پایدار Workflow است.

---

## مکانیزم داخلی

1. Worker به Cluster درخواست Start Timer می‌دهد
2. Cluster Timer را ثبت می‌کند
3. پس از اتمام مدت:
   - یک Workflow Task جدید داخل Task Queue قرار می‌گیرد
4. Worker اجرای Workflow را ادامه می‌دهد

نکته مهم:
بTimer حتی اگر هیچ Workerای در حال اجرا نباشد، Fire می‌شود.


## لینک‌ها


[[Using Timers in Workflow Code]]

[[Use Cases for Timers]]



---

## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود


