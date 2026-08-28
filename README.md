<div align="center">

# 🏠 Smart Home Automation System
### ARM-Based Multi-Modal Home Control Using STM32 Blue Pill

[![STM32](https://img.shields.io/badge/MCU-STM32F103C8T6-blue?style=flat-square&logo=stmicroelectronics)](https://www.st.com/)
[![Language](https://img.shields.io/badge/Language-C-lightgrey?style=flat-square&logo=c)](https://en.wikipedia.org/wiki/C_(programming_language))
[![IDE](https://img.shields.io/badge/IDE-STM32CubeIDE-blue?style=flat-square)](https://www.st.com/en/development-tools/stm32cubeide.html)
[![Course](https://img.shields.io/badge/Course-CSE%20331%20Embedded%20Systems-orange?style=flat-square)](https://www.northsouth.edu/)
[![University](https://img.shields.io/badge/University-North%20South%20University-green?style=flat-square)](https://www.northsouth.edu/)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

<br/>

> **Wireless Bluetooth-based appliance control** combined with **automatic sensor-driven actuation** — all on a sub-$20 embedded platform with zero cloud dependency.

<br/>

<!-- Replace with your actual project photo -->
![Project Prototype](images/project_photo.jpg)

*Fig. 1 — Hardware prototype of the Smart Home Automation System*

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [System Architecture](#-system-architecture)
- [Hardware Components](#-hardware-components)
- [Pin Configuration](#-pin-configuration)
- [Mathematical Models](#-mathematical-models)
- [Firmware Algorithm](#-firmware-algorithm)
- [Performance Results](#-performance-results)
- [Project Timeline](#-project-timeline)
- [Budget](#-budget)
- [Team](#-team)
- [Future Work](#-future-work)
- [References](#-references)

---

## 🔍 Overview

This project presents the design and implementation of a **Smart Home Automation System** built on the **STM32F103C8T6 (Blue Pill)** ARM Cortex-M3 microcontroller. The system provides:

- **Manual control** of 4 AC appliances via Bluetooth from a mobile app
- **Automatic fan activation** when room temperature exceeds 30 °C (DHT11)
- **Automatic light switching** triggered by IR proximity detection
- **Security alerting** via buzzer on motion detection
- **Real-time display** of sensor readings and system status

The system operates **fully offline** — no Wi-Fi, no cloud, no network dependency — making it ideal for homes, university labs, and resource-constrained environments.

| Metric | Target | Achieved |
|---|---|---|
| Bluetooth response latency | < 1 s | ✅ < 1 s |
| DHT11 temperature accuracy | ±2 °C | ✅ ±2 °C |
| IR light response | < 0.5 s | ✅ Immediate |
| Relay switching reliability | ≥ 90% | ✅ **95%** |
| Bluetooth range | 10 m | ✅ Up to 10 m |
| Controllable appliances | 4 | ✅ 4 |

---

## ✨ Features

- 📱 **Bluetooth manual control** — send single-character commands from any Android app via HC-05
- 🌡️ **Auto fan control** — DHT11 continuously monitors temperature; fan turns ON automatically above 30 °C
- 👁️ **IR auto light** — infrared proximity sensor drives the light relay with no user interaction
- 🔔 **Buzzer security alert** — activated by IR motion detection when security mode is active
- 🖥️ **LED display** — shows real-time sensor readings and system state
- ⚡ **4-channel relay** — safely switches 230 V AC loads with optocoupler isolation
- 🔋 **Sub-$20 total cost** — BDT 2,210 (~$20 USD) for complete hardware
- 🌐 **IoT-ready architecture** — modular design ready for ESP32/Wi-Fi/Firebase upgrade

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        INPUT LAYER                              │
│  📱 Mobile App  ──BT──►  📡 HC-05   ──UART──►                 │
│  👁️  IR Sensor  ──GPIO──────────────────────►  STM32F103C8T6  │
│  🌡️  DHT11      ──1-Wire────────────────────►  (Blue Pill)    │
└─────────────────────────────────────────────────────────────────┘
                              │ PROCESSING LAYER
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       OUTPUT LAYER                              │
│  STM32  ──GPIO──►  ⚡ Relay Module  ──►  💡 AC Appliances      │
│  STM32  ──GPIO──►  🔔 Buzzer                                    │
│  STM32  ──GPIO──►  🖥️  LED Display                             │
└─────────────────────────────────────────────────────────────────┘
```

<!-- Replace with your actual block diagram image -->
![Block Diagram](images/block_diagram.png)

*Fig. 2 — System Architecture Block Diagram*

---

## 🔧 Hardware Components

| Component | Model | Qty | Cost (BDT) |
|---|---|:---:|:---:|
| Microcontroller | STM32F103C8T6 (Blue Pill) | 1 | 350 |
| Programmer | ST-LINK V2 | 1 | 400 |
| Bluetooth Module | HC-05 | 1 | 300 |
| Relay Module | 4-Channel (Active LOW) | 1 | 250 |
| IR Sensor | Infrared Proximity Sensor | 1 | 80 |
| Temperature Sensor | DHT11 | 1 | 100 |
| Buzzer | Active Buzzer | 1 | 30 |
| Display | LED Display Module | 1 | 200 |
| Mobile Application | Bluetooth Controller App | 1 | — |
| Power Supply | 5V DC Regulated | 1 | 300 |
| Breadboard, wires, misc. | — | — | 200 |
| **Total** | | | **2,210 BDT (~$20 USD)** |

---

## 📌 Pin Configuration

### HC-05 Bluetooth Module

| HC-05 Pin | STM32 Pin | Purpose |
|---|---|---|
| TXD | PA10 (USART1_RX) | Send data to STM32 |
| RXD | PA9 (USART1_TX) | Receive data from STM32 |
| VCC | 5V | Power supply |
| GND | GND | Ground |

> **Protocol:** UART/USART1 · **Baud rate:** 9600 bps · **Frame:** 8N1

### Relay Module

| Channel | STM32 Pin | Function |
|---|---|---|
| IN1 | PB9 | Automatic Light (IR sensor) |
| IN2 | PB8 | Fan (DHT11 auto + mobile control) |
| IN3 | PB10 | Mobile App Control |
| IN4 | PB11 | Future Expansion / IoT |
| VCC | 5V | Power supply |
| GND | GND | Ground |

> **Logic:** Active LOW — `GPIO RESET` → Relay ON · `GPIO SET` → Relay OFF

### Sensors & Buzzer

| Component | STM32 Pin | Protocol |
|---|---|---|
| IR Sensor OUT | PA0 | GPIO Digital IN |
| DHT11 DATA | PA1 | Single-wire (1-Wire) |
| Buzzer Signal | PB12 | GPIO Digital OUT |

<!-- Replace with your actual circuit schematic image -->
![Circuit Schematic](images/circuit_schematic.png)

*Fig. 3 — Complete Circuit Schematic*

---

## 📐 Mathematical Models

### 1. UART Baud Rate Generation

$$B = \frac{f_{CK}}{16 \times \text{USARTDIV}}$$

For `f_CK = 8 MHz`, `B = 9600 bps` → `USARTDIV = 52.08`

Each 8N1 frame (10 bits) duration: **T_frame ≈ 1.04 ms** — well within the 10 ms HAL timeout.

### 2. Relay Active LOW Logic

```
R_i = NOT(G_i)    where G_i ∈ {0=RESET, 1=SET}
```

Fan relay combines Bluetooth command and temperature condition:

```
R_fan = C_BT  OR  C_temp
C_temp = 1  if T > 30°C
         0  if T ≤ 30°C
```

### 3. DHT11 Data Decoding

40-bit frame: `[RH_int | RH_dec | T_int | T_dec | Checksum]`

```
Checksum = (RH_int + RH_dec + T_int + T_dec) mod 256
T   = T_int + T_dec/10   [°C]
RH  = RH_int + RH_dec/10 [%]
```

### 4. IR Detection Range

Detection occurs when reflected intensity exceeds threshold:

```
d_max = sqrt( (η × I_e) / (4π × I_th) )
```

Empirically validated detection range: **5–15 cm** depending on surface reflectivity.

---

## ⚙️ Firmware Algorithm

```
Algorithm: Smart Home Automation Firmware Control Loop

INITIALIZE:
  HAL, System Clock, GPIO, USART1
  Set all relays OFF (GPIO_PIN_SET, active LOW)

LOOP (while system active):

  [1] Bluetooth Control (USART1)
      Receive 1 byte via USART1 (timeout: 10 ms)
      '1' → PB8  RESET  (Fan ON)
      '0' → PB8  SET    (Fan OFF)
      '2' → PB10 RESET  (Appliance 3 ON)
      '3' → PB10 SET    (Appliance 3 OFF)
      '4' → PB11 RESET  (Appliance 4 ON)
      '5' → PB11 SET    (Appliance 4 OFF)

  [2] IR Sensor → Auto Light
      Read PA0
      if PA0 = LOW  → PB9 RESET  (Light ON)
      else          → PB9 SET    (Light OFF)

  [3] DHT11 → Auto Fan Override
      Decode temperature T
      if T > 30°C   → PB8 RESET  (Fan ON override)

END LOOP
```

---

## 📊 Performance Results

| Parameter | Target | Measured | Status |
|---|---|---|---|
| UART frame duration | < 5 ms | ≈ 1.04 ms | ✅ Pass |
| Bluetooth response latency | < 1 s | < 1 s | ✅ Pass |
| DHT11 temperature accuracy | ±2 °C | ±2 °C | ✅ Pass |
| Auto fan threshold | 30 °C | 30 °C | ✅ Pass |
| IR light response | < 0.5 s | Immediate | ✅ Pass |
| Relay switching reliability | ≥ 90% | **95%** | ✅ Pass |
| Bluetooth range | 10 m | Up to 10 m | ✅ Pass |
| Controllable appliances | 4 | 4 | ✅ Pass |

> 100 consecutive Bluetooth commands transmitted with **zero byte errors**, validating 8N1 framing at 9600 bps.

---

## 📅 Project Timeline

| Phase | Duration | Milestones |
|---|---|---|
| Phase 1 | Week 1–3 | Component procurement, circuit assembly, sensor wiring, STM32CubeMX configuration |
| Phase 2 | Week 4–6 | HAL firmware development: UART control, relay logic, DHT11 decoding, IR integration |
| Phase 3 | Week 7–8 | Integrated system testing, performance benchmarking, report writing |

---

## 🛠️ How to Build & Flash

### Prerequisites

- [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html) (v1.13+)
- ST-LINK V2 programmer
- Android device with a Bluetooth serial terminal app

### Steps

```bash
# 1. Clone this repository
git clone https://github.com/souravnewbee/Smart-Home-stm32-bluepill.git
cd Smart-Home-stm32-bluepill

# 2. Open in STM32CubeIDE
#    File → Open Projects from File System → select this folder

# 3. Build
#    Project → Build All  (Ctrl+B)

# 4. Flash to Blue Pill via ST-LINK V2
#    Run → Debug (F11)  or  Run → Run (Ctrl+F11)
```

### Bluetooth Pairing

1. Power on the system — HC-05 LED will blink rapidly
2. On your Android device → Settings → Bluetooth → Pair `HC-05`
3. Default PIN: **1234** (or **0000**)
4. Open a Bluetooth serial terminal app and connect
5. Send commands: `1` (Fan ON) · `0` (Fan OFF) · `2`/`3` (Appliance 3) · `4`/`5` (Appliance 4)

---

## 👥 Team

### 🔧 Sourav Roy — Lead Engineer 

> Primary contributor responsible for the majority of the hardware and technical implementation.

- ✅ Full circuit design, schematic, and breadboard assembly
- ✅ GPIO & UART wiring — STM32 ↔ HC-05, relay module, IR sensor, DHT11, buzzer
- ✅ DHT11 single-wire protocol implementation & microsecond timing calibration
- ✅ Temperature threshold logic (30 °C auto fan activation)
- ✅ IR sensor integration with 50 ms software debounce
- ✅ Relay active-LOW switching logic & power rail isolation
- ✅ Sensor calibration against reference thermometer
- ✅ End-to-end hardware validation and troubleshooting
- ✅ GitHub repository setup and project documentation

---

### 🤝 Other Contributors

| Name | Student ID | Contribution |
|---|---|---|
| **Ahnaf Mohammed Mahi Kabir** | STM32CubeMX peripheral configuration, HAL-based C firmware, control loop structure |
| **Afia Mubassira Ali Raisa** | Android Bluetooth pairing with HC-05, real-time command testing via mobile app |
| **Khatune Jannat** | IEEE report formatting and technical writing |

---

**Department of Electrical and Computer Engineering**
**North South University, Dhaka, Bangladesh**
**Course: CSE 331 — Embedded Systems · Spring 2026**

---

## 🚀 Future Work

- [ ] **Wi-Fi / IoT upgrade** — replace HC-05 with ESP32 for MQTT-based cloud control via Firebase or ThingSpeak
- [ ] **PID fan speed control** — closed-loop PWM-based fan speed modulation replacing current bang-bang threshold
- [ ] **Camera security** — add edge-based intrusion detection inspired by deep-learning security models
- [ ] **Gas & flame detection** — safety monitoring with local buzzer and cloud alerts
- [ ] **Energy monitoring** — ACS712 current sensors on each relay channel for real-time power logging
- [ ] **Voice control** — Google Assistant / Alexa integration via ESP32 cloud bridge

---

## 📚 References

1. ARM Holdings, "Cortex-M3 Technical Reference Manual," Rev. r2p1, 2010.
2. STMicroelectronics, "STM32F103x8 Datasheet," Rev. 17, 2015.
3. STMicroelectronics, "STM32F103 Reference Manual (RM0008)," Rev. 21, 2015.
4. Aosong Electronics, "DHT11 Humidity and Temperature Sensor Datasheet," 2010.
5. A. F. H. Dhrubo and M. A. Qayum, "STM32-Based IoT Framework for Real-Time Environmental Monitoring," arXiv:2506.17295, 2025.
6. L. Gao and G. Li, "Design and Implementation of Smart Home Control System Based on STM32," WCNA 2023, Springer LNEE Vol. 1361, pp. 112–125, 2025.
7. O. Taiwo et al., "Enhanced Intelligent Smart Home Control and Security System Based on Deep Learning Model," Wireless Communications and Mobile Computing, Wiley, 2022.
8. "Next-Gen Home Automation: Sensor-Based Connectivity and Appliance Control," Springer, 2024.

---

<div align="center">

Made with ❤️ at **North South University** · Spring 2026

⭐ Star this repo if it helped you!

</div>