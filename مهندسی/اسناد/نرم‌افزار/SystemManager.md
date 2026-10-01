---
type: firmware
module: system-manager
version: "1.0.0"
status: implemented
date: 2026-09-24
tags: [firmware, system, manager]
---

# SystemManager

**فایل:** `src/system/SystemManager.cpp` / `.h`
**نسخه:** 1.0.0

## وظیفه

مدیریت کل سیستم — راه‌اندازی و چرخه اجرای تمام ماژول‌ها.

## متدها

### `begin()`

به ترتیب زیر تمام ماژول‌ها را راه‌اندازی می‌کند:

1. `Hardware::begin()` — مقداردهی پین‌های خروجی
2. `state.begin()` — مقداردهی اولیه [[StateManager]]
3. `devices.begin()` — راه‌اندازی [[DeviceManager]]
4. `sensors.begin()` — راه‌اندازی [[SensorManager]]
5. `automation.begin()` — راه‌اندازی [[AutomationManager]]
6. `network.begin()` — اتصال WiFi و NTP
7. `blynk.begin()` — پیکربندی [[BlynkManager]]
8. `webUI.begin()` — راه‌اندازی [[WebUIManager]]
9. `registerBlynkHandlers()` — فعال‌سازی دکمه‌های دستی Blynk

### `update()`

چرخه مداوم — به ترتیب زیر فراخوانی می‌کند:

1. `network.update()`
2. `devices.update()`
3. `sensors.update()`
4. `blynk.update()`
5. `automation.update()`
6. `webUI.update()`
7. `state.update()`

## نکته

این کلاس هیچ منطق تجاری ندارد — فقط هماهنگ‌کننده‌ی سایر ماژول‌هاست.

## مرتبط

- [[Firmware-Architecture]]
- [[StateManager]]
- [[DeviceManager]]
- [[SensorManager]]
- [[AutomationManager]]
- [[BlynkManager]]
- [[WebUIManager]]
