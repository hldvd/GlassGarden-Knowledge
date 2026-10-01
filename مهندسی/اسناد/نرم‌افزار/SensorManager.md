---
type: firmware
module: sensor-manager
version: "1.3.0"
status: implemented
date: 2026-09-24
tags: [firmware, sensor, dht22, water-level]
---

# SensorManager

**فایل:** `src/sensors/SensorManager.cpp` / `.h`
**نسخه:** 1.3.0

## وظیفه

خواندن دما و رطوبت از سنسور DHT22 و سطح آب از فلوتر سوئیچ، و ذخیره در [[StateManager]].

## متدها

### `begin()`

- `dht.begin()` — راه‌اندازی سنسور DHT
- تنظیم پین `WATER_PIN` (GPIO4) به‌عنوان `INPUT_PULLUP`
- `state.lastValidSensorReadMs = millis()` — ثبت زمان اولیه

### `update()`

هر `SENSOR_READ_INTERVAL` (۲۵۰۰ میلی‌ثانیه) یک بار اجرا می‌شود.

#### ۱) خواندن فلوتر سوئیچ (سطح آب)

```
HIGH = کلید باز = آب نیست → waterLevelPercent = 0
LOW  = کلید بسته = آب هست → waterLevelPercent = 100
```

تغییر وضعیت آب خالی با لاگ سریال همراه است:
- ورود به حالت خالی: `[Sensor] Water EMPTY! Float switch open.`
- خروج از حالت خالی: `[Sensor] Water present. Float switch closed.`

#### ۲) خواندن DHT22 (دما و رطوبت)

```cpp
float h = dht.readHumidity();
float t = dht.readTemperature();
```

اگر خواندن ناموفق باشد (`isnan`)، مقدار قبلی حفظ می‌شود و `lastValidSensorReadMs` به‌روز نمی‌شود. این برای تشخیص قطعی طولانی سنسور و ورود به حالت ایمن (Safe Mode) استفاده می‌شود. → [[AutomationManager]]

## نمونه سراسری

```cpp
extern SensorManager sensors;
```

## مرتبط

- [[Firmware-Architecture]]
- [[DHT22-Sensor]]
- [[Water-Level-Sensor]]
- [[StateManager]]
- [[AutomationManager]] — استفاده از `lastValidSensorReadMs` برای Safe Mode
