---
type: hardware
component: sensor
model: float-switch
gpio: 4
status: final
date: 2026-09-24
tags: [hardware, sensor, water-level, float-switch]
---

# سنسور سطح آب — فلوتر سوئیچ

## مشخصات

- نوع: فلوتر سوئیچ (دو سیمه)
- پین: GPIO4
- منطق: دیجیتال با `INPUT_PULLUP` داخلی

## نحوه کار

| وضعیت | کلید | پین | `waterLevelPercent` |
|---|---|---|---|
| آب موجود | بسته | `HIGH` | ۱۰۰ |
| آب خالی | باز | `LOW` | ۰ |

> منطق کد `SensorManager.cpp`: `digitalRead(WATER_PIN) == HIGH` یعنی آب موجود.

کلاس `SensorManager` در هر چرخه خواندن، وضعیت فلوتر سوئیچ را می‌خواند. تغییر از «موجود» به «خالی» باعث فعال شدن هشدار `waterEmpty = true` می‌شود.

## محافظت در برابر آب خالی

وقتی `waterEmpty = true` باشد:
- `DeviceManager::update()` پمپ و مه‌ساز را فوراً خاموش می‌کند.
- `AutomationManager` هم پمپ و مه‌ساز را روشن نمی‌کند.

این محافظت در هر دو حالت AUTO و MANUAL فعال است.

## مرتبط

- [[GPIO-Map]]
- [[SensorManager]]
- [[DeviceManager]]
- [[Water-Level-Sensor-Inconsistency|⚠️ ناسازگاری با Config]]
