​مفهوم: اینترنت گاهی پورت ۲۲ (SSH) را مسدود می‌کند.
​نشانه: خطای Connection closed یا Could not resolve hostname.
​راه حل: تست با ssh -T git@github.com. اگر وصل نشد، فیلترشکن یا DNS را چک کن.
​اشتباه دی‌باگ: دستکاری فایل ~/.ssh/config بدون داشتن DNS درست، باعث کور شدن گیت می‌شود.