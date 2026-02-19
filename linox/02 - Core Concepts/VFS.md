---
created: 2026-02-18
updated: 2026-02-18 23:31
tags: []
---
# {{VFS}}

VFS = Virtual File System
یک لایه داخل کرنل لینوکس که:
بین برنامه‌ها و فایل‌سیستم‌های واقعی قرار می‌گیرد
یک API استاندارد برای کار با فایل فراهم می‌کند
یعنی برنامه فقط این‌ها را می‌شناسد:


open()
read()
write()
close()
ولی اصلاً مهم نیست:
ext4 باشد
xfs باشد
ntfs باشد
network fs باشد
usb باشد
ram باشد

___
) جریان واقعی از دید سیستم
وقتی این را اجرا می‌کنی:

cat file.txt
اتفاق واقعی:
Copy code

User Program (cat)
      ↓
System Call (read)
      ↓
VFS
      ↓
Filesystem Driver (ext4 / xfs / ntfs)
      ↓
Disk
VFS مثل «مدیر ترافیک» عمل می‌کند.
## لینک‌ها
[[Relationship between vfs and mount]]



---

## 🔄 Processing Status
- [ ] #to-process   ← خام (هنوز فکر اولیه است)
- [ ] #to-link      ← هنوز به نوت‌های دیگر لینک نشده
- [ ] #to-expand    ← نیاز به تکمیل و توضیح بیشتر
- [ ] #to-test      ← باید در عمل تست شود
- [ ] #to-review    ← نیاز به بازبینی و اصلاح