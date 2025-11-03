# Smart Footwear – IoT Based Assistive Walking System

[![Status](https://img.shields.io/badge/status-active-brightgreen)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](#license)
[![Platform](https://img.shields.io/badge/platform-Arduino%20%7C%20ESP8266%20%7C%20IoT-orange)](#software-components)
[![Made with Love](https://img.shields.io/badge/Made%20with-%E2%9D%A4%EF%B8%8F-red)](#)

TS. Srinivasan Polytechnic College • Vanagaram, Chennai, Tamil Nadu 600095  
Guide: Mrs. S. P. Chitra (HOD)

---

## 📘 Overview
Smart Footwear is an IoT-based assistive walking system designed to enhance mobility and safety for visually impaired individuals. It integrates ultrasonic sensing, haptic feedback, and optional audio alerts to detect obstacles and provide timely feedback during indoor and outdoor navigation.

- Problem: Difficulty detecting foot-level and mid-height obstacles causing accidents.
- Objective: Build an affordable, compact, shoe-mounted system with multi-zone detection and feedback.
- Impact: Improves safety, autonomy, and confidence for daily mobility.

---

## ⚙️ Key Features
| # | Feature | Description |
|---|---|---|
| 1 | Multi-Zone Obstacle Detection | Dual ultrasonic sensors for near (toe) and mid-level (knee) detection |
| 2 | Haptic Feedback | Vibration motor intensity scales with obstacle proximity |
| 3 | Audio Alerts | Optional buzzer/earpiece feedback for critical warnings |
| 4 | Smart Thresholds | Configurable distance thresholds with debounce filtering |
| 5 | Low Power | Sleep mode and efficient power management |
| 6 | IoT Connectivity | ESP8266/ESP32 for logging and optional remote assistance |
| 7 | Environmental Sensing | Optional LDR/IMU for ambient light and tilt detection |
| 8 | Compact & Wearable | Lightweight, shoe-mounted enclosure |
| 9 | Safety First | Short-circuit and polarity protection |

---

## 🧑‍🤝‍🧑 Team
| Name | Role | Email |
|---|---|---|
| Student A | Hardware & PCB | studentA@example.com |
| Student B | Embedded & Firmware | studentB@example.com |
| Student C | Cloud & App | studentC@example.com |
| Student D | Testing & Documentation | studentD@example.com |

> Replace placeholder emails with your actual contacts before publishing.

---

## 🏗️ System Architecture

<details>
<summary>View ASCII Architecture Diagram</summary>

```
+-----------------------------+          +-----------------------+
|  Left Shoe Module           |          |  Right Shoe Module    |
|  (HC-SR04, Vib Motor, MCU)  |          |  (HC-SR04, Vib Motor) |
+--------------+--------------+          +------------+----------+
               |                                  |
               | I2C / GPIO                       | GPIO
               v                                  v
          +-----------+                     +-----------+
          |  MCU      |<----UART/WiFi-----> |  ESP8266  |
          | (ATmega/  |                     |  (Cloud)  |
          |  ESP32)   |                     +-----------+
          +-----+-----+                            |
                |                                  |
                v                                  v
         +-------------+                    +--------------+
         | Haptic PWM  |                    | Cloud/MQTT   |
         |  Driver     |                    | (Optional)   |
         +-------------+                    +--------------+
```
</details>

![System Architecture](architecture.png#gh-dark-mode-only)
![System Architecture](architecture.png#gh-light-mode-only)

---

## 🔌 Circuit Diagram

<details>
<summary>View ASCII Circuit Sketch</summary>

```
HC-SR04(TRIG,ECHO) --> D5, D6
Vibration Motor --> NPN Transistor -> D9 (PWM), +5V
Buzzer (opt) --> D10
ESP8266 UART --> MCU TX/RX (3.3V level shift)
Power --> 18650 + Step-Down (5V/3.3V rails)
```
</details>

![Circuit Diagram](circuit_diagram.png#gh-dark-mode-only)
![Circuit Diagram](circuit_diagram.png#gh-light-mode-only)

---

## 🧩 Bill of Materials (BOM)
| Item | Qty | Notes |
|---|---:|---|
| HC-SR04 Ultrasonic Sensor | 2 | Toe and knee-level detection |
| ATmega328P or ESP32 | 1 | Controller |
| ESP8266 (NodeMCU) | 1 | Optional IoT connectivity |
| Vibration Motor | 1 | Haptic feedback |
| NPN Transistor + Diode | 1 set | Motor driver |
| Buzzer (optional) | 1 | Critical alerts |
| Li-ion 18650 + BMS | 1 | Power |
| Step-Down Regulators | 2 | 5V and 3.3V |
| Wires, Shoe Enclosure | — | Mounting |

---

## 🧠 Working Principle
1. Distance is sampled from ultrasonic sensors at 10–20 Hz.
2. Filtered via moving average and debounce to eliminate false triggers.
3. Haptic intensity maps to distance bands (closer = stronger vibration).
4. Critical zone triggers audio alert and optional cloud event.
5. Sleep/wake states optimize battery life when idle.

---

## 🧪 Results & Testing
- Detection range: 5–250 cm (configurable)
- Average response latency: < 50 ms
- Field tests show improved obstacle avoidance at foot and mid-level heights.

---

## 📂 Repository Tree
```
.
├── firmware/
│   ├── src/
│   │   └── main.ino
│   └── lib/
├── hardware/
│   ├── pcb/
│   └── enclosure/
├── docs/
│   ├── architecture.png
│   └── circuit_diagram.png
└── README.md
```

---

## 🛠️ Quick Start
```bash
# Firmware (Arduino/PlatformIO)
platformio run -t upload
# or Arduino IDE: open firmware/src/main.ino and upload
```

Example Arduino sketch:
```cpp
#include <Arduino.h>
const int trig = 5, echo = 6, vib = 9;
long readCM() {
  digitalWrite(trig, LOW); delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long dur = pulseIn(echo, HIGH, 30000);
  return dur / 58; // microseconds to cm
}
void setup(){ pinMode(trig, OUTPUT); pinMode(echo, INPUT); pinMode(vib, OUTPUT); }
void loop(){
  long d = readCM();
  int pwm = d < 30 ? map(d, 5, 30, 255, 0) : 0;
  analogWrite(vib, constrain(pwm, 0, 255));
  delay(40);
}
```

---

## 🌐 Software Components
- Firmware: Arduino (ATmega328P/ESP32)
- Optional: ESP8266 for MQTT/HTTP telemetry
- Cloud: Any MQTT broker (Mosquitto), dashboard (Node-RED/Grafana)

---

## 🚀 Future Enhancements
- BLE smartphone app with voice guidance
- IMU-based step and fall detection
- LiPo fuel gauging and auto-calibrated thresholds
- 3D-printed ergonomic enclosure

---

## 🙏 Acknowledgments
- TS. Srinivasan Polytechnic College (Support & Lab access)
- Guide: Mrs. S. P. Chitra (HOD)
- Open-source community and assistive tech research papers

---

## 📜 License

Released under the MIT License. See [LICENSE](LICENSE) for details.

---

## 🔗 Direct Links
- Project Page: https://github.com/Darkwebnew/DESIGN-AND-MODELLING-OF-FOOTWEAR-FOR-VISUALLY-IMPAIRED
- Issues: https://github.com/Darkwebnew/DESIGN-AND-MODELLING-OF-FOOTWEAR-FOR-VISUALLY-IMPAIRED/issues
- Pull Requests: https://github.com/Darkwebnew/DESIGN-AND-MODELLING-OF-FOOTWEAR-FOR-VISUALLY-IMPAIRED/pulls

![circuit_diagram](https://github.com/user-attachments/assets/d68f0efc-a715-4be6-a7db-7c08c03f1737)
![architecture](https://github.com/user-attachments/assets/71cfbc07-00d5-43b2-a939-7b68e5a4bb73)
