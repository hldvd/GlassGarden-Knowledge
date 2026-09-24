---
type: firmware
module: blynk-manager
version: "1.0.0"
status: implemented
date: 2026-09-24
tags: [firmware, blynk, cloud, iot]
---

# BlynkManager

**فایل:** `src/blynk/BlynkManager.cpp` / `.h` + `BlynkHandlers.cpp` + `BlynkHandlersHook.h`
**نسخه:** 1.0.0 (BlynkHandlers: 1.1.3)

## وظیفه

مدیریت ارتباط با Blynk Cloud — دریافت فرمان‌ها و ارسال داده‌های سنسور.

## راه‌اندازی

- `Blynk.config(BLYNK_AUTH_TOKEN)` — پیکربندی بدون اتصال فوری
- `BLYNK_HEARTBEAT = 30` — فاصله heartbeat از ۱۰ به ۳۰ ثانیه افزایش یافته

## چرخه ارسال داده‌ها (Staggered)

به جای ارسال همه Virtual Pin ها پشت سر هم، هر ۸۰۰ms یک `virtualWrite` انجام می‌شود:

| Step | Datastream | داده |
|---|---|---|
| 0 | V1 | دما |
| 1 | V2 | رطوبت |
| 2 | V0 | وضعیت نور |
| 3 | V3 | وضعیت مه‌ساز |
| 4 | V4 | وضعیت فن |
| 5 | V5 | وضعیت پمپ |
| 6 | V7 | سطح آب (٪) |

## BlynkHandlers — دریافت فرمان‌ها

فایل `BlynkHandlers.cpp` فرمان‌های دریافتی از اپلیکیشن Blynk را پردازش می‌کند:

| Virtual Pin | تجهیز | عمل |
|---|---|---|
| V0 | نور | `devices.lightOn()/lightOff()` |
| V3 | مه‌ساز | `devices.foggerOn()/foggerOff()` |
| V4 | فن | `devices.fanOn()/fanOff()` |
| V5 | پمپ | `devices.pumpOn()/pumpOff()` |
| V6 | حالت | `state.autoMode = val` (AUTO/MANUAL) |

هر فرمان با لاگ timestamp و RSSI ثبت می‌شود:
```
[Blynk-CMD] t=12345 ms | RSSI=-65 dBm | V0 Light = 1
```

## همگام‌سازی وضعیت (`syncState`)

پس از اتصال موفق، وضعیت فعلی تمام تجهیزات و سنسورها به Blynk ارسال می‌شود.

## اتصال مجدد

- اگر WiFi قطع باشد: `blynkConnected = false`
- اگر Blynk قطع باشد: هر ۵ ثانیه تلاش اتصال مجدد با `Blynk.connect(1000)`

## نکته لینکر

تابع `registerBlynkHandlers()` در `BlynkHandlers.cpp` عمداً خالی است — فقط برای جلوگیری از حذف فایل توسط لینکر (رفع مشکل عدم‌اجرای فرمان‌های دستی).

## مرتبط

- [[Firmware-Architecture]]
- [[Config-and-Datastreams]] — تعریف Virtual Pin ها
- [[DeviceManager]] — فرمان‌دهی تجهیزات
- [[StateManager]] — خواندن وضعیت و همگام‌سازی
