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

# May 31, 2025 (2 hours)
## Finalizing the Display
After discussing with others and thinking a bit more, I ended up choosing the Waveshare display because I want my experience to be smooth. This is basically my first time doing hardware so I don't want to dive into the deep end just yet. With the BuyDisplay display I might even have to write my own touch driver.

**Final choice:** Waveshare 1.28inch Round LCD Display Module with Touch panel

## Choosing an RTC module
I'm just going to go with the module used by ZSWatch. It looks pretty good and I don't really want time mulling over RTC modules.
**Final choice:** [RV-8263-C8](https://www.microcrystal.com/en/products/real-time-clock-rtc-modules/rv-8263-c8)

## Choosing a Flash IC
The ZSWatch uses the MX25U51245G and I don't really have much of a preference for flash ICs, so I'll just use it.
**Final choice:** MX25U51245G

## Choosing Sensors
Since I wanted an accelerometer, gyroscope, and magnetometer, I was thinking about going with the LSM9DS1 since it conveniently packages all the sensors I need in one IC. However, it uses a minimum of 1.9 mA while in its always-on eco mode, which seems a bit excessive. After thinking a bit and looking at other sensor options, I realized that it would be better to go with a 6 axis IMU and use a separate magnetometer, since I'll really only be using the magnetometer while in the compass app of the watch. With the magnetometer separated, I can completely cut off its power when I'm not using it, while still keeping the accelerometer and gyroscope active for listening to things like gestures.

I looked around for a while and eventually chose the BMI270 for the 6 axis IMU and the BMM150 for the magnetometer. While there were other choices for the IMU, like the LSM6DSOX with its embedded AI capabilities, I decided the BMI270 was simpler and more than enough, as it has a built-in set of gestures and motions it can detect. I don't really remember why I chose the BMM150 but apparently it has good support and is small and capable so it should be good.

**Final choice:** BMI270 + BMM150

## Ending Notes
There are still some components I need to choose but I might begin designing the PCB soon. I have absolutely no experience in PCB design so it'll probably take a while and I might need to change my component choice later on anyways.
