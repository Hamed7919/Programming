## 🧱 مرحله 3 — فهم ساختار واقعی پروژه Go

اینجا تفاوت مبتدی و حرفه‌ای شروع میشه.

پروژه واقعی این شکلی نمی‌مونه 👇

`temporal-hello/  ├─ go.mod  ├─ main.go`

بلکه تبدیل میشه به:
`temporal-hello/`
`│`
`├─ cmd/`
`│   └─ app/`
`│       └─ main.go`
`│`
`├─ internal/`
`│   ├─ handler/`
`│   ├─ service/`
`│   ├─ repository/`
`│   └─ domain/`
`│`
`├─ pkg/`
`├─ configs/`
`└─ go.mod`
