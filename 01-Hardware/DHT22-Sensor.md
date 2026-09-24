---
type: hardware
component: sensor
model: DHT22 (AM2302)
gpio: 17
status: final
date: 2026-09-24
tags: [hardware, sensor, dht22, temperature, humidity]
---

# سنسور DHT22 — دما و رطوبت

## مشخصات

- مدل: DHT22 / AM2302
- نوع: دیجیتال، تک‌سیمه
- بازه دما: ۴۰- تا ۸۰+ درجه سانتی‌گراد
- بازه رطوبت: ۰ تا ۱۰۰٪
- پین: GPIO17 (با pull-up ده کیلواهم)

## نحوه کار

کلاس `SensorManager` (در `src/sensors/SensorManager.cpp`) هر ۲۵۰۰ میلی‌ثانیه یک بار دما و رطوبت را می‌خواند و در [[StateManager]] ذخیره می‌کند.

- ثابت `DHT_TYPE = 22` در Config
- کتابخانه: `adafruit/DHT sensor library`
- اگر خواندن ناموفق باشد (`isnan`)، مقدار قبلی حفظ می‌شود و `lastValidSensorReadMs` به‌روز نمی‌شود.

## اهمیت در اتوماسیون

زمان آخرین خواندن معتبر (`lastValidSensorReadMs`) برای تشخیص قطعی طولانی سنسور استفاده می‌شود. اگر بیش از ۲ دقیقه خواندن معتبر نباشد، سیستم وارد حالت ایمن (Safe Mode) می‌شود. → [[AutomationManager]]

## مرتبط

- [[GPIO-Map]]
- [[SensorManager]]
- [[Config-and-Datastreams]]
