---
type: hardware
component: sensor
model: P100
gpio: 4
status: final
date: 2026-09-24
tags: [hardware, sensor, water-level, p100, analog]
---

# سنسور سطح آب — P100 آنالوگ

## مشخصات

- نوع: سنسور آنالوگ سطح آب P100
- پین: GPIO4 (ADC1)
- منطق: خواندن مقدار خام ADC با `analogRead`

## ثابت‌های کالیبراسیون

| ثابت | مقدار | توضیح |
|---|---|---|
| `WATER_LEVEL_EMPTY` | ۵۰۰ | مقدار خام ADC هنگام خالی بودن مخزن |
| `WATER_LEVEL_FULL` | ۳۵۰۰ | مقدار خام ADC هنگام پر بودن مخزن |
| `WATER_LEVEL_EMPTY_HYSTERESIS` | ۱۵۰ | حاشیه هیسترزیس برای خروج از حالت «خالی» (جلوگیری از نوسان) |

## نحوه کار مورد انتظار

مقدار خام ADC خوانده می‌شود و با `map` به درصد تبدیل می‌شود:

```cpp
int raw = analogRead(WATER_PIN);
int percent = map(raw, WATER_LEVEL_EMPTY, WATER_LEVEL_FULL, 0, 100);
```

هیسترزیس برای جلوگیری از نوسان سریع هشدار «آب خالی» وقتی مقدار خام نزدیک مرز است.

## محافظت در برابر آب خالی

وقتی `waterEmpty = true` باشد:
- `DeviceManager::update()` پمپ و مه‌ساز را فوراً خاموش می‌کند.
- `AutomationManager` هم پمپ و مه‌ساز را روشن نمی‌کند.

این محافظت در هر دو حالت AUTO و MANUAL فعال است.

> ⚠️ **توجه:** کد فعلی `SensorManager.cpp` به‌جای `analogRead` از `digitalRead` (فلوتر سوئیچ) استفاده می‌کند که با سخت‌افزار واقعی P100 مطابقت ندارد. → [[Water-Level-Sensor-Inconsistency]]

## مرتبط

- [[GPIO-Map]]
- [[SensorManager]]
- [[DeviceManager]]
- [[Config-and-Datastreams]]
- [[Water-Level-Sensor-Inconsistency]]
