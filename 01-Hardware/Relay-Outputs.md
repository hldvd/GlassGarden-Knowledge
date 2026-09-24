---
type: hardware
component: relay-module
model: JQC-3F-5VDC-C
gpio: [26, 27, 32, 33]
status: final
date: 2026-09-24
tags: [hardware, relay, power]
---

# ماژول رله‌ها

## مشخصات

- مدل: JQC-3F-5VDC-C
- ولتاژ کاری: ۵ ولت
- منطق: Active-LOW (`LOW` = روشن، `HIGH` = خاموش)
- تعداد کانال: ۴ (پمپ، نور، مه‌ساز، فن)

## پین‌ها

| خروجی | GPIO | ثابت در Config |
|---|---|---|
| پمپ | GPIO26 | `PUMP_PIN` |
| نور | GPIO27 | `LIGHT_PIN` |
| مه‌ساز | GPIO32 | `FOGGER_PIN` |
| فن | GPIO33 | `FAN_PIN` |

## نحوه کار

کلاس `Outputs` (در `src/hardware/Outputs.cpp`) مسئول نوشتن مستقیم روی پین‌هاست. تابع `writeOutput` بر اساس `OUTPUT_ACTIVE_HIGH` تصمیم می‌گیرد که `HIGH`/`LOW` بنویسد. هر بار که خروجی تغییر می‌کند، لاگ سریال چاپ می‌شود:

```
[Output] Light (GPIO 27) -> ON
```

## راه‌اندازی اولیه

در `Hardware::initializePins()` تمام پین‌های خروجی روی `OUTPUT` تنظیم و در حالت خاموش (`HIGH` به‌دلیل Active-LOW) قرار می‌گیرند.

## مرتبط

- [[GPIO-Map]]
- [[Power-System]]
- [[Config-and-Datastreams]]
