---
title: "Smartwatch"
author: "Eason"
description: "A custom smartwatch inspired by ZSWatch"
created_at: "2025-05-29"
---

# May 29, 2025
Spent a while looking at ZSWatch for inspiration. I also created the goal/feature list, weighing complexity, monetary cost, and power cost of the features I thought of. I think they're roughly in order of importance.

Now that I think about it the first two "goals" are kind of a must so don't really count.
## Goals / Features
- Custom PCB
- Custom case (Will probably be 3D printed)
- Touchscreen
- Wifi + Bluetooth capabilities
- USB-C port
- Side buttons
- Accelerometer
- Gyroscope
- Magnetometer
- Rechargeable battery
- ~64 MB of storage
- App system

# May 30, 2025 (2 hours)
## Choosing a Microcontroller
Preferably the MCU should have Wifi and Bluetooth capabilities built in so I can achieve wireless capabilities easily. Also, it should have a decent speed (for fast rendering and smooth experience for apps). Since I'll likely be directly connecting to the internet through Wifi, I want hardware accelerated encryption so I'm not wasting CPU resources on encryption.

Being popular and well documented is important too! As is being able to be programmed in Rust because that's my favourite low level language :)

I think the ESP32-S3 is a pretty good fit. It checks all of the aforementioned requirements. I'm not very sure about the speed part but 240MHz is probably enough. There appears to be good Rust support - [esp-rs](https://docs.esp-rs.org/book/) even provides `std`! ESP32s are apparently quite power hungry when fully active, so I'll need to be careful about power management (turning off radio when unneeded, using sleep modes). If I run out of pins or processing power I might switch to a STM32N6 (I'll need to use separate wireless modules though).

**Final choice:** ESP32-S3

## Choosing a Display
I'm still debating between these two displays:
- https://www.waveshare.com/1.28inch-touch-lcd.htm
- https://www.buydisplay.com/1-28-inch-tft-lcd-display-240x240-round-circle-screen-for-smart-watch

The BuyDisplay display is a lot cheaper but the documentation seems poor (It doesn't even say what touch driver it uses! Also, while the Waveshare display has examples for the Raspberry Pi Pico and STM32s, the BuyDisplay one has a single small example for the 8051 microcontroller). Also, the BuyDisplay display has a flex cable instead of just exposing pins, so that might be troublesome. I'm going to ask some friends for advice and come back to this tomorrow.
