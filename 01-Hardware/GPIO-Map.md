---
type: hardware
component: gpio-map
status: final
date: 2026-09-24
tags: [hardware, gpio, esp32]
---

# نقشه پین‌های GPIO

نقشه تخصیص پین‌های میکروکنترلر ESP32 (مدل `esp32dev`).

## پین‌های خروجی (رله‌ها — Active LOW)

| قطعه | پین GPIO | توضیح |
|---|---|---|
| پمپ آب | GPIO26 | رله ۱ |
| نور LED | GPIO27 | رله ۲ |
| مه‌ساز (فوگر) | GPIO32 | رله ۳ |
| فن | GPIO33 | رله ۴ |

> رله‌ها Active-LOW هستند: `LOW` = روشن، `HIGH` = خاموش.
> ثابت `OUTPUT_ACTIVE_HIGH = false` در [[Config-and-Datastreams]].

## پین‌های سنسور

| قطعه | پین GPIO | توضیح |
|---|---|---|
| DHT22 | GPIO17 | سنسور دما و رطوبت (با pull-up ده کیلواهم) |
| سنسور سطح آب | GPIO4 | فلوتر سوئیچ (دو سیمه) |

> پین سطح آب با `INPUT_PULLUP` داخلی راه‌اندازی شده است.
> ⚠️ منطق سطح آب در کد: `HIGH` = آب موجود، `LOW` = آب خالی.
> به کد `SensorManager.cpp` مراجعه کنید: `digitalRead(WATER_PIN) == HIGH` یعنی آب موجود.

## مرتبط

- [[Relay-Outputs]]
- [[DHT22-Sensor]]
- [[Water-Level-Sensor]]
- [[Config-and-Datastreams]]
