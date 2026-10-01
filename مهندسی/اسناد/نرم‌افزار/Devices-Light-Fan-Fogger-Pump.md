---
type: firmware
module: devices
version: "1.0.0"
status: implemented
date: 2026-09-24
tags: [firmware, devices, light, fan, fogger, pump]
---

# کلاس‌های تجهیزات — Light / Fan / Fogger / Pump

**فایل‌ها:** `src/devices/LightDevice.*` / `FanDevice.*` / `FoggerDevice.*` / `PumpDevice.*`
**نسخه:** 1.0.0

## ساختار مشترک

هر چهار کلاس ساختار یکسانی دارند:

```cpp
class XxxDevice {
public:
    void begin();    // فراخوانی off() در زمان راه‌اندازی
    void on();       // Outputs::xxx(true) + state = true
    void off();      // Outputs::xxx(false) + state = false
    void toggle();   // تغییر وضعیت
    bool isOn() const;
private:
    bool state = false;
};
```

## نحوه کار

هر کلاس فقط یک لایه‌ی انتزاعی بالای `Outputs` است:

| کلاس | فراخوانی Outputs | GPIO |
|---|---|---|
| `LightDevice` | `Outputs::light()` | GPIO27 |
| `FanDevice` | `Outputs::fan()` | GPIO33 |
| `FoggerDevice` | `Outputs::fogger()` | GPIO32 |
| `PumpDevice` | `Outputs::pump()` | GPIO26 |

هر فرمان `on()`/`off()` دو کار انجام می‌دهد:
1. فراخوانی `Outputs::xxx(state)` → نوشتن روی پین GPIO
2. به‌روزرسانی متغیر `state` داخلی کلاس

> توجه: `DeviceManager` علاوه بر فراخوانی این کلاس‌ها، وضعیت را در [[StateManager]] هم به‌روز می‌کند.

## مرتبط

- [[DeviceManager]] — مدیر مرکزی تجهیزات
- [[Relay-Outputs]] — کلاس Outputs و ماژول رله
- [[Config-and-Datastreams]] — تعریف پین‌ها
- [[Firmware-Architecture]]
