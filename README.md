# 🚗 Line Following & Gantry Detection Robot – Arduino Project

## 📌 Overview

This project implements an **autonomous robot** using an Arduino board. The robot:

* Detects and counts **gantry crossings** using a pulse signal.
* Uses an **ultrasonic sensor** for obstacle detection.
* Uses **two IR sensors (A0 & A1)** for line following.
* Controls movement using a **motor driver (pins 5–8)**.
* Starts operation when it receives `'C'` from Serial Monitor.

---

## 🧰 Hardware Requirements

* Arduino Board (e.g., **Arduino Uno**)
* Ultrasonic Sensor (e.g., **HC-SR04**)
* Motor Driver (e.g., **L298N**)
* 2 × IR Sensors (Line detection)
* DC Motors
* Power supply
* Connecting wires

---

## 🔌 Pin Configuration

| Component           | Arduino Pin |
| ------------------- | ----------- |
| Ultrasonic TRIG     | 13          |
| Ultrasonic ECHO     | 12          |
| Gantry Input Signal | 4           |
| Motor Control Pins  | 5, 6, 7, 8  |
| Left IR Sensor      | A0          |
| Right IR Sensor     | A1          |

---

## ⚙️ How It Works

### 1️⃣ Gantry Detection (Pin 4)

* Reads pulse width using `pulseIn()`.
* Depending on pulse value, it identifies:

  * **500–1000** → Gantry 1 Crossed
  * **2500–3000** → Gantry 3 Crossed
  * **3200–3700** → Gantry 2 Crossed
* Stops for 1 second after detection.

---

### 2️⃣ Start Condition

Robot starts when:

```cpp
Serial.read() == 'C'
```

Once started:

* `flag = 1`
* Robot enters autonomous mode.

---

### 3️⃣ Obstacle Detection

Using ultrasonic sensor:

```cpp
distanceCm = (duration * 0.034) / 2;
```

* If distance > 10 cm → Continue moving
* If distance ≤ 10 cm → Stop

---

### 4️⃣ Line Following Logic

Using IR sensors:

| Left (A0) | Right (A1) | Action                       |
| --------- | ---------- | ---------------------------- |
| 1         | 1          | Forward                      |
| 1         | 0          | Left                         |
| 0         | 1          | Right                        |
| 0         | 0          | Time-based movement sequence |

---

### 5️⃣ Time-Based Path Control

If both sensors detect no line:

* Every 1 second → `count++`
* Robot performs predefined movement sequence:

| Count | Action       |
| ----- | ------------ |
| 1     | Forward      |
| 2     | Right        |
| 3     | Forward      |
| 4     | Forward      |
| 5     | Right        |
| 6     | Forward      |
| >6    | Stop & Reset |

---

## 🚦 Movement Functions

The robot movement is controlled using:

* `forward()`
* `backward()`
* `left()`
* `right()`
* `clockwise()`
* `counterclockwise()`
* `stopp()`

Each function sets motor driver pins HIGH/LOW accordingly.

---

## 🖥️ Serial Output

The Serial Monitor (9600 baud) displays:

* Pulse value from gantry sensor
* Gantry crossed message
* Count value during timed movement

---

## ▶️ How to Use

1. Connect all hardware as per pin configuration.
2. Upload the code to Arduino.
3. Open Serial Monitor (9600 baud).
4. Send character **`C`** to start the robot.
5. Robot will:

   * Detect gantries
   * Follow line
   * Avoid obstacles
   * Execute timed path

---

## 📌 Notes

* Make sure ultrasonic sensor faces forward.
* IR sensors must be calibrated for proper line detection.
* Power supply should be stable to avoid motor glitches.
* Delay-based turning may require tuning depending on motor speed.

---

## 📄 Summary

This project combines:

* Line following
* Obstacle avoidance
* Pulse-based gantry detection
* Time-sequenced navigation

It is suitable for:

* Robotics competitions
* Smart gantry tracking systems
* Autonomous navigation experiments

---
