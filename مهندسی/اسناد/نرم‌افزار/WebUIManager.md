---
type: firmware
module: webui-manager
version: "1.1.0"
status: implemented
date: 2026-09-24
tags: [firmware, web-ui, websocket, esp32]
---

# WebUIManager

**فایل:** `src/webui/WebUIManager.cpp` / `.h` + `WebUIHtml.h` + `WebUICss.h` + `Images.h`
**نسخه:** 1.1.0

## وظیفه

راه‌اندازی سرور وب محلی روی ESP32، ارتباط real-time با WebSocket، و کنترل تجهیزات و نمایش سنسورها بدون وابستگی به Blynk.

## API Endpoints

### `GET /`

صفحه HTML اصلی — شامل CSS و تصاویر (لوگو و پس‌زمینه) به‌صورت inline.

### `GET /api/state`

بازگشت وضعیت کامل سیستم به‌صورت JSON:

```json
{
  "temperature": 25.5,
  "humidity": 60.0,
  "waterLevelPercent": 100,
  "waterEmpty": false,
  "light": false,
  "fan": false,
  "fogger": false,
  "pump": false,
  "autoMode": true,
  "safeMode": false,
  "wifiConnected": true,
  "blynkConnected": true
}
```

### `POST /api/control`

کنترل تجهیزات — فقط در حالت MANUAL:
```json
{"type": "cmd", "device": "fan", "value": true}
```

اگر `autoMode = true` باشد، فرمان وب نادیده گرفته می‌شود.

## WebSocket

- مسیر: `/ws`
- رویداد `connect`: ارسال وضعیت فعلی به کلاینت جدید (`broadcastState`)
- رویداد `data`: پردازش فرمان JSON (مشابه `POST /api/control`)
- رویداد `disconnect`: لاگ قطع اتصال

## همگام‌سازی

`broadcastState()` وضعیت فعلی را به تمام کلاینت‌های متصل WebSocket ارسال می‌کند.

## امنیت

- کپی امن داده WebSocket (جلوگیری از buffer overflow)
- بررسی null-pointer برای فیلدهای JSON
- استفاده از `portMUX_TYPE` برای thread-safety در دسترسی به `state`

## مرتبط

- [[Firmware-Architecture]]
- [[StateManager]]
- [[DeviceManager]]
- [[AutomationManager]]
