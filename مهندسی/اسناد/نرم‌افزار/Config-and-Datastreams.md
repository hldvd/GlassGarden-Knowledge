---
type: firmware
module: config-datastreams
version: "1.1.0"
status: implemented
date: 2026-09-24
tags: [firmware, config, datastreams, blynk-vpin]
---

# Config و Datastreams

**فایل‌ها:** `src/core/Config.h` + `src/core/Datastreams.h` + `src/core/ConfigLocal.h.example`
**نسخه:** 1.1.0

## Config.h — پیکربندی اصلی

### اطلاعات پروژه

- `PROJECT_NAME` = "GlassGarden"
- `PROJECT_VERSION` = "1.1.0"
- `SERIAL_BAUDRATE` = 115200

### GPIO — خروجی‌ها (رله‌ها)

| ثابت | GPIO | قطعه |
|---|---|---|
| `PUMP_PIN` | 26 | پمپ آب |
| `LIGHT_PIN` | 27 | نور LED |
| `FOGGER_PIN` | 32 | مه‌ساز |
| `FAN_PIN` | 33 | فن |

### GPIO — سنسورها

| ثابت | GPIO | قطعه |
|---|---|---|
| `DHT_PIN` | 17 | DHT22 |
| `WATER_PIN` | 4 | فلوتر سوئیچ |

### منطق رله

- `OUTPUT_ACTIVE_HIGH = false` (Active-LOW: `LOW` = روشن)

### سطح آب

| ثابت | مقدار | توضیح |
|---|---|---|
| `WATER_LEVEL_EMPTY` | 500 | آستانه ADC خالی |
| `WATER_LEVEL_FULL` | 3500 | آستانه ADC پر |
| `WATER_LEVEL_EMPTY_HYSTERESIS` | 150 | هیسترزیس خروج از حالت خالی |

> ⚠️ این ثابت‌ها برای سنسور آنالوگ P100 طراحی شده‌اند، اما کد فعلی از فلوتر سوئیچ دیجیتال استفاده می‌کند. → [[Water-Level-Sensor-Inconsistency]]

### اتوماسیون — آستانه‌ها

| تجهیز | شرط روشن | شرط خاموش |
|---|---|---|
| فن (دما) | ≥ ۲۸°C | ≤ ۲۶°C |
| فن (رطوبت) | ≥ ۷۵٪ | ≤ ۷۰٪ |
| مه‌ساز (رطوبت) | ≤ ۶۰٪ | ≥ ۷۰٪ |

### اتوماسیون — زمان‌بندی

| تجهیز | روشن | خاموش |
|---|---|---|
| نور | ساعت ۸ | ساعت ۲۰ |
| فن | ساعت ۸ | ساعت ۲۰ |
| مه‌ساز | ساعت ۶ | ساعت ۲۲ |
| پمپ | ساعت ۸ | ساعت ۲۰ |

### NTP

- سرور: `pool.ntp.org`
- GMT Offset: ۱۲۶۰۰ ثانیه (۳:۳۰ — ایران)
- Daylight Offset: ۰

## Datastreams.h — Virtual Pin های Blynk

### خروجی‌ها

| ثابت | Pin | تجهیز |
|---|---|---|
| `VPIN_LIGHT` | V0 | نور |
| `VPIN_FOGGER` | V3 | مه‌ساز |
| `VPIN_FAN` | V4 | فن |
| `VPIN_PUMP` | V5 | پمپ |

### سنسورها

| ثابت | Pin | داده |
|---|---|---|
| `VPIN_TEMPERATURE` | V1 | دما |
| `VPIN_HUMIDITY` | V2 | رطوبت |

### حالت

| ثابت | Pin | داده |
|---|---|---|
| `VPIN_AUTO_MODE` | V6 | سوییچ AUTO/MANUAL |
| `VPIN_WATER_LEVEL` | V7 | سطح آب (٪) |

## ConfigLocal.h

فایل `ConfigLocal.h.example` نمونه‌ای برای تنظیمات محلی است که شامل `BLYNK_AUTH_TOKEN` و احتمالاً `WIFI_SSID`/`WIFI_PASSWORD` می‌شود. این فایل در `.gitignore` قرار دارد.

## مرتبط

- [[Firmware-Architecture]]
- [[GPIO-Map]]
- [[AutomationManager]] — استفاده از آستانه‌ها و زمان‌بندی‌ها
- [[BlynkManager]] — استفاده از Virtual Pin ها
