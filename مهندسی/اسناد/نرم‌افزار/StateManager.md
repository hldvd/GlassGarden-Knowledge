---
type: firmware
module: state-manager
version: "1.1.0"
status: implemented
date: 2026-09-24
tags: [firmware, state, manager]
---

# StateManager

**فایل:** `src/state/StateManager.cpp` / `.h`
**نسخه:** 1.1.0

## وظیفه

نگهداری وضعیت لحظه‌ای سیستم. تمام بخش‌های نرم‌افزار فقط از این کلاس وضعیت را می‌خوانند یا تغییر می‌دهند.

## متغیرهای وضعیت

### تجهیزات

| متغیر | نوع | توضیح |
|---|---|---|
| `light` | `bool` | وضعیت نور LED |
| `fan` | `bool` | وضعیت فن |
| `fogger` | `bool` | وضعیت مه‌ساز |
| `pump` | `bool` | وضعیت پمپ |

### سنسورها

| متغیر | نوع | توضیح |
|---|---|---|
| `temperature` | `float` | دما (سانتی‌گراد) |
| `humidity` | `float` | رطوبت (٪) |
| `waterLevelPercent` | `int` | سطح آب (٪) — ۰ یا ۱۰۰ |
| `waterEmpty` | `bool` | هشدار آب خالی (با هیسترزیس) |
| `lastValidSensorReadMs` | `unsigned long` | زمان آخرین خواندن معتبر DHT |
| `safeMode` | `bool` | حالت ایمن — قطعی طولانی DHT |

### ارتباط

| متغیر | نوع | توضیح |
|---|---|---|
| `wifiConnected` | `bool` | وضعیت اتصال WiFi |
| `blynkConnected` | `bool` | وضعیت اتصال Blynk |

### حالت اتوماسیون

| متغیر | نوع | توضیح |
|---|---|---|
| `autoMode` | `bool` | `true` = AUTO، `false` = MANUAL |

## نمونه سراسری

```cpp
extern StateManager state;
```

در سراسر کد با نام `state` قابل دسترسی است.

## مرتبط

- [[Firmware-Architecture]]
- [[SensorManager]] — نوشتن مقادیر سنسور
- [[DeviceManager]] — نوشتن وضعیت تجهیزات
- [[AutomationManager]] — نوشتن `safeMode` و خواندن وضعیت
- [[BlynkManager]] — همگام‌سازی وضعیت با Blynk
