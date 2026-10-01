---
type: firmware
module: architecture
status: implemented
date: 2026-09-24
tags: [firmware, architecture, overview]
---

# معماری نرم‌افزار GlassGarden

نسخه: 1.1.0 | فریم‌ورک: Arduino / PlatformIO | میکروکنترلر: ESP32

## نمودار ماژول‌ها

```
main.cpp
  └── SystemManager
        ├── Hardware          → مقداردهی اولیه پین‌ها
        ├── StateManager      → وضعیت لحظه‌ای سیستم
        ├── SensorManager     → خواندن DHT22 + فلوتر سوئیچ
        ├── DeviceManager     → کنترل Light/Fan/Fogger/Pump
        │     ├── LightDevice
        │     ├── FanDevice
        │     ├── FoggerDevice
        │     └── PumpDevice
        ├── AutomationManager → تصمیم‌گیری خودکار (دما/رطوبت/زمان)
        ├── NetworkManager    → WiFi (WiFiManager) + NTP
        ├── BlynkManager      → ارتباط با Blynk Cloud
        │     └── BlynkHandlers → دریافت فرمان‌های Blynk
        └── WebUIManager      → سرور وب محلی + WebSocket
```

## چرخه اجرا

### `setup()` — راه‌اندازی اولیه

1. `Serial.begin(115200)`
2. `registerBlynkHandlers()` — جلوگیری از حذف فایل توسط لینکر
3. `SystemManager::begin()` — راه‌اندازی تمام ماژول‌ها به ترتیب:
   - `Hardware::begin()` → مقداردهی پین‌ها
   - `state.begin()` → مقداردهی اولیه وضعیت
   - `devices.begin()` → راه‌اندازی تجهیزات (همگی خاموش)
   - `sensors.begin()` → راه‌اندازی DHT و فلوتر سوئیچ
   - `automation.begin()` → ریست تایمرهای تغییر وضعیت
   - `network.begin()` → اتصال WiFi + همگام‌سازی NTP
   - `blynk.begin()` → پیکربندی Blynk
   - `webUI.begin()` → راه‌اندازی سرور وب

### `loop()` — چرخه مداوم

`SystemManager::update()` به ترتیب زیر فراخوانی می‌کند:
1. `network.update()` — بررسی اتصال WiFi
2. `devices.update()` — محافظت آب خالی
3. `sensors.update()` — خواندن سنسورها (هر ۲.۵ ثانیه)
4. `blynk.update()` — اجرای Blynk + ارسال داده‌ها (هر ۸۰۰ms)
5. `automation.update()` — تصمیم‌گیری خودکار
6. `webUI.update()` — پاک‌سازی کلاینت‌های قطع شده WebSocket
7. `state.update()` — به‌روزرسانی وضعیت

## کتابخانه‌های خارجی

| کتابخانه | کاربرد |
|---|---|
| `blynkkk/Blynk` | ارتباط با Blynk Cloud |
| `adafruit/DHT sensor library` | خواندن سنسور DHT22 |
| `adafruit/Adafruit Unified Sensor` | پیش‌نیاز DHT |
| `ESPAsyncWebServer` | سرور وب غیرهمزمان |
| `AsyncTCP` | پیش‌نیاز ESPAsyncWebServer |
| `ArduinoJson` | پردازش JSON در Web API |
| `tzapu/WiFiManager` | پیکربندی WiFi از طریق پورتال |

## ماژول‌های نرم‌افزار

- [[SystemManager]] — مدیر اصلی
- [[StateManager]] — وضعیت لحظه‌ای
- [[SensorManager]] — خواندن سنسورها
- [[DeviceManager]] — کنترل تجهیزات
- [[AutomationManager]] — تصمیم‌گیری خودکار
- [[BlynkManager]] — ارتباط با Blynk
- [[WebUIManager]] — رابط وب محلی
- [[Config-and-Datastreams]] — پیکربندی و Virtual Pin ها
- [[Devices-Light-Fan-Fogger-Pump]] — کلاس‌های تجهیزات
