# Track Following Bot

An autonomous track-following robot that uses IR sensors and a simple control algorithm to detect and follow a predefined track, while also sensing nearby obstacles.

---

## Features

- Follows a black or white line track using HW-201 sensors.
- Can detect nearby obstacles with additional self-soldered IR sensors.
- Controlled by an Arduino Nano microcontroller.

---

## Components Used

| Component            | Quantity | Details                                             |
|----------------------|----------|-----------------------------------------------------|
| Arduino              | 1        | Microcontroller to control the bot                  |
| L298N Motor Controller | 1        | Dual H-Bridge motor driver for DC motors            |
| DC Motors            | 2        | Drive wheels                                        |
| Wheels               | 2        | 1 normal wheel pair + 1 castor wheel for support    |
| Power Source         | 1        | 7V rechargeable batteries                           |
| IR Proximity Sensors | 4 (HW-201: in diamond pattern for track sensing), 
2 (self-soldered: for obstacle detection)|
---

## How It Works

1. **Track Sensing:**
   - 4 IR sensors are mounted in a diamond pattern at the front of the bot to detect the track.
   - Sensors detect the contrast between the line and the background.
   - The Arduino processes sensor data to adjust motor speeds and keep the bot on track.

2. **Obstacle Detection:**  
   - 2 additional IR sensors are mounted for detecting nearby obstacles.

3. **Movement:**  
   - Two DC motors drive the main wheels.
   - The L298N motor controller interfaces the Arduino and the motors.
   - A castor wheel helps balance the bot while allowing smooth turns.

---

## Power

- Powered by a 7V rechargeable battery pack which supplies the Arduino, sensors, and motors.

---
