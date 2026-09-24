---
type: firmware
module: device-manager
version: "1.2.0"
status: implemented
date: 2026-09-24
tags: [firmware, devices, manager]
---

# DeviceManager

**فایل:** `src/devices/DeviceManager.cpp` / `.h`
**نسخه:** 1.2.0

## وظیفه

مدیریت تمام تجهیزات پروژه. تمام فرمان‌های مربوط به تجهیزات ابتدا به این کلاس ارسال می‌شوند و سپس به کلاس مربوط به هر تجهیز منتقل می‌شوند.

## متدها

### `begin()`

راه‌اندازی چهار تجهیز:
- `lightDevice.begin()` → `off()`
- `fanDevice.begin()` → `off()`
- `foggerDevice.begin()` → `off()`
- `pumpDevice.begin()` → `off()`

### `update()`

**محافظت آب خالی** — در هر حالت (AUTO/MANUAL):
- اگر `waterEmpty` و `fogger` روشن → `foggerOff()` + لاگ
- اگر `waterEmpty` و `pump` روشن → `pumpOff()` + لاگ

### کنترل هر تجهیز

برای هر تجهیز چهار متد وجود دارد:

| متد | Light | Fan | Fogger | Pump |
|---|---|---|---|---|
| روشن کردن | `lightOn()` | `fanOn()` | `foggerOn()` | `pumpOn()` |
| خاموش کردن | `lightOff()` | `fanOff()` | `foggerOff()` | `pumpOff()` |
| تغییر وضعیت | `lightToggle()` | `fanToggle()` | `foggerToggle()` | `pumpToggle()` |
| وضعیت فعلی | `lightState()` | `fanState()` | `foggerState()` | `pumpState()` |

هر متد ابتدا فرمان را به کلاس تجهیز مربوطه می‌فرستد و سپس وضعیت را در [[StateManager]] به‌روز می‌کند.

### `allOff()`

خاموش کردن همزمان تمام تجهیزات.

## کلاس‌های تجهیزات

هر کلاس (`LightDevice`, `FanDevice`, `FoggerDevice`, `PumpDevice`) ساختار یکسانی دارد و فقط `Outputs` را فراخوانی می‌کند. → [[Devices-Light-Fan-Fogger-Pump]]

## نمونه سراسری

```cpp
extern DeviceManager devices;
```

## مرتبط

- [[Firmware-Architecture]]
- [[StateManager]]
- [[Devices-Light-Fan-Fogger-Pump]]
- [[AutomationManager]] — فرمان‌دهی خودکار تجهیزات
- [[BlynkManager]] — فرمان‌دهی از طریق Blynk
- [[Water-Level-Sensor]] — محافظت آب خالی
