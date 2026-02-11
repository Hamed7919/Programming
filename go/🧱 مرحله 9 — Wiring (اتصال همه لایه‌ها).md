## 🧱 مرحله 9 — Wiring (اتصال همه لایه‌ها)

اینجا Dependency Injection اتفاق می‌افتد.

در main:

`repo := repository.NewUserRepo() service := service.NewUserService(repo) handler := handler.NewUserHandler(service)`

---

