---
type: problem
status: open
date: 2026-09-24
related: [[Config-and-Datastreams]], [[Water-Level-Sensor]], [[SensorManager]]
tags: [problem, inconsistency, water-level, sensor]
---

# ناسازگاری سنسور سطح آب — Config در برابر کد

**وضعیت:** باز
**تاریخ:** 2026-09-24
**مربوط به:** [[Config-and-Datastreams]]، [[Water-Level-Sensor]]، [[SensorManager]]

## شرح مشکل

در `Config.h` ثابت‌های مربوط به سنسور سطح آب آنالوگ (P100) تعریف شده‌اند:

```cpp
constexpr uint16_t WATER_LEVEL_EMPTY = 500;
constexpr uint16_t WATER_LEVEL_FULL  = 3500;
constexpr uint16_t WATER_LEVEL_EMPTY_HYSTERESIS = 150;
```

اما در `SensorManager.cpp` (نسخه ۱.۳.۰) سنسور سطح آب به‌عنوان **فلوتر سوئیچ دیجیتال** با `INPUT_PULLUP` پیاده‌سازی شده است:

```cpp
pinMode(WATER_PIN, INPUT_PULLUP);
bool waterPresent = (digitalRead(WATER_PIN) == HIGH);
state.waterLevelPercent = waterPresent ? 100 : 0;
```

این یعنی:
- ثابت‌های `WATER_LEVEL_EMPTY`، `WATER_LEVEL_FULL` و `WATER_LEVEL_EMPTY_HYSTERESIS` **استفاده نمی‌شوند**.
- کامنت `Config.h` هنوز به «سنسور سطح آب P100» اشاره دارد.
- منطق هیسترزیس در `SensorManager` به‌صورت دیجیتال پیاده شده (تغییر وضعیت با لاگ سریال).
- منطق سطح آب در کد: `HIGH` = آب موجود، `LOW` = آب خالی.

## سوالات نیازمند تأیید

۱. آیا سنسور از P100 آنالوگ به فلوتر سوئیچ دیجیتال تغییر کرده؟ اگر بله، ثابت‌های Config باید پاک‌سازی شوند.
۲. آیا هنوز پلن استفاده از سنسور آنالوگ P100 در آینده وجود دارد؟ اگر بله، ثابت‌ها باید با کامنت «رزرو برای آینده» مشخص شوند.

## اقدام پیشنهادی

- پاک‌سازی یا کامنت‌گذاری ثابت‌های مرتبط با ADC در `Config.h`
- به‌روزرسانی کامنت `WATER_PIN` از «سنسور سطح آب P100» به «فلوتر سوئیچ دیجیتال»
- به‌روزرسانی [[GlassGarden-Master]] پس از تصمیم‌گیری
