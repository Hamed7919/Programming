---
created: 2026-02-24
updated: 2026-02-24 09:59
tags: []
---
# تشخیص و مدیریت خطاهای کانتکست

برای رفتار درست سیستم، باید بدانیم چرا کانتکست متوقف شده است:

- ب`context.Canceled`: کاربر یا سیستم به صورت دستی `cancel()` را صدا زده است.
- ب`context.DeadlineExceeded`: زمان تعیین شده (Timeout) به پایان رسیده است.
```
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