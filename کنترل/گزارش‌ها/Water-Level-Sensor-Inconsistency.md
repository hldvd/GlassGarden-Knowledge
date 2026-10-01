---
type: problem
status: open
date: 2026-09-24
related: [[Config-and-Datastreams]], [[Water-Level-Sensor]], [[SensorManager]]
tags: [problem, bug, water-level, sensor, p100]
---

# ناسازگاری سنسور سطح آب — کد در برابر سخت‌افزار واقعی

**وضعیت:** باز
**تاریخ:** 2026-09-24
**مربوط به:** [[Config-and-Datastreams]]، [[Water-Level-Sensor]]، [[SensorManager]]

## شرح مشکل

سخت‌افزار واقعی پروژه از سنسور **آنالوگ P100** برای اندازه‌گیری سطح آب استفاده می‌کند. ثابت‌های مربوطه در `Config.h` به‌درستی تعریف شده‌اند:

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
- خواندن دیجیتال (`digitalRead`) به‌جای آنالوگ (`analogRead`) انجام می‌شود.
- `waterLevelPercent` فقط ۰ یا ۱۰۰ است، به‌جای درصد واقعی.
- منطق هیسترزیس آنالوگ تعریف شده اما استفاده نمی‌شود.

## اقدام لازم

کد `SensorManager.cpp` باید به‌روزرسانی شود تا از `analogRead(WATER_PIN)` با ثابت‌های P100 استفاده کند:

۱. `pinMode(WATER_PIN, INPUT)` به‌جای `INPUT_PULLUP`
۲. خواندن مقدار خام ADC: `int raw = analogRead(WATER_PIN);`
۳. محاسبه درصد: `map(raw, WATER_LEVEL_EMPTY, WATER_LEVEL_FULL, 0, 100)`
۴. اعمال هیسترزیس برای خروج از حالت «خالی» با `WATER_LEVEL_EMPTY_HYSTERESIS`
۵. پین `WATER_PIN` (GPIO4) باید برای ورودی آنالوگ (ADC1) مناسب باشد

## مرتبط

- [[Config-and-Datastreams]] — ثابت‌های P100
- [[Water-Level-Sensor]] — باید به P100 آنالوگ به‌روزرسانی شود
- [[SensorManager]] — کد نیازمند اصلاح
- [[GlassGarden-Master]] — پس از رفع مشکل به‌روزرسانی شود
