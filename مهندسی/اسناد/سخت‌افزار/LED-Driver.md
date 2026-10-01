---
type: hardware
component: led-driver
model: LM317T
status: final
date: 2026-09-24
tags: [hardware, led, driver, lm317]
---

# درایور LED

## مشخصات

- آی‌سی: LM317T
- نوع: مدار جریان ثابت (Constant Current)
- جریان خروجی: ۸۳۳ میلی‌آمپر

## نحوه اتصال

درایور LED از طریق ماژول رله (GPIO27) کنترل می‌شود. وقتی رله روشن است، مدار LM317T تغذیه را به LED می‌رساند و جریان ثابت ۸۳۳ میلی‌آمپر تضمین می‌شود.

## مرتبط

- [[Relay-Outputs]]
- [[Power-System]]
- [[GPIO-Map]]
