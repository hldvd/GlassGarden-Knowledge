---
type: hardware
component: power
status: final
date: 2026-09-24
tags: [hardware, power, psu, buck]
---

# سیستم تغذیه

## زنجیره تغذیه

```
آداپتور 12V/5A → مبدل Buck LM2596 → 5V → ESP32 + رله‌ها + سنسورها
```

## مشخصات

| بخش | مشخصات |
|---|---|
| آداپتور | ۱۲ ولت، ۵ آمپر |
| مبدل Buck | LM2596 → خروجی ۵ ولت |
| رله‌ها | ۵ ولت، Active-LOW |
| ESP32 | تغذیه ۵ ولت (از طریق پین 5V/VIN) |

## مرتبط

- [[Relay-Outputs]]
- [[LED-Driver]]
