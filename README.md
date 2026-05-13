[README.md](https://github.com/user-attachments/files/27703555/README.md)
# Blood-Pressure-Monitor
Reverse-engineered embedded blood pressure monitor using ESP32 and Oscillometric Method
# 🩺 Reverse-Engineered Blood Pressure Monitor

An embedded blood pressure monitoring system built from scratch using the **Oscillometric Method** — going beyond a basic sensor interface to full system integration.

![Status](https://img.shields.io/badge/Status-Prototype%20Functional-green)
![Platform](https://img.shields.io/badge/Platform-ESP32-blue)
![PCB](https://img.shields.io/badge/PCB-Altium%20Designer-A5915F)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## 📌 Overview

This project implements a non-invasive blood pressure measurement system using the **oscillometric technique** — the same method used in commercial automatic BP monitors. The system inflates a cuff, then detects micro-oscillations in cuff pressure as blood pulses through the artery during deflation. Systolic and diastolic pressures are derived from the oscillation envelope.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────┐
│                    Power Stage                       │
│  USB-C ──► Battery Charger ──► Boost Converter      │
│                                ──► LDO 3.3V          │
└─────────────────────────────────────────────────────┘
                          │
┌─────────────────────────────────────────────────────┐
│               Processing & Sensing                   │
│                                                      │
│   Pressure Sensor ──► HX710 (24-bit ADC)             │
│                              │                       │
│                          ESP32                       │
│                              │                       │
│         OLED (I2C) ◄─────────┤                       │
│         Push Buttons ────────┤                       │
│         Air Pump ◄───────────┤  (Inflation)          │
│         Solenoid Valve ◄─────┘  (Deflation)          │
└─────────────────────────────────────────────────────┘
```

---

## 🔧 Hardware Components

| Block | Component | Description |
|-------|-----------|-------------|
| **MCU** | ESP32 | Main processing unit |
| **ADC** | HX710 | 24-bit ADC for micro-pressure variation capture |
| **Sensor** | Pressure Sensor | Cuff pressure measurement |
| **Power** | USB-C + Battery Charger | Portable power input |
| **Power** | Boost Converter | Voltage step-up for pump |
| **Power** | LDO 3.3V | Clean regulated supply for analog/digital circuits |
| **Actuation** | Air Pump | Cuff inflation |
| **Actuation** | Solenoid Valve | Controlled cuff deflation |
| **UI** | OLED Display (I2C) | Real-time pressure & result display |
| **UI** | Push Buttons | User input & measurement trigger |

---

## 📁 Repository Structure

```
bp-monitor/
├── firmware/
│   ├── main/
│   │   ├── main.c               # Application entry point
│   │   └── CMakeLists.txt
│   └── components/
│       ├── pressure_sensor/     # HX710 ADC driver
│       ├── oled_display/        # I2C OLED driver
│       ├── pump_control/        # Pump & solenoid control
│       └── ble/                 # BLE server (future)
├── hardware/
│   ├── schematic/               # Altium schematic files
│   ├── pcb/                     # Altium PCB layout files
│   └── bom/                     # Bill of Materials
├── docs/
│   ├── images/                  # Photos, diagrams, screenshots
│   └── datasheets/              # Component datasheets
├── tests/
│   └── test_oscillometric.c     # Algorithm validation tests
├── .gitignore
└── README.md
```

---

## ⚡ Real-World Challenges & Solutions

### 1. Boost Converter Noise Corrupting ADC Signals
- **Problem:** High-frequency switching noise from the boost converter was coupling into the analog signal path, corrupting the HX710 ADC readings.
- **Solution:** PCB layout isolation — separated analog and digital ground planes with a single-point star connection. Added LC filtering on the power rails feeding the analog section.

### 2. PCB Layout — Analog/Digital Track Isolation
- **Problem:** Digital switching currents (ESP32, pump driver) inducing noise on sensitive analog traces.
- **Solution:** Careful placement with analog components grouped together, away from high-current switching paths. Used poured copper ground fills on separate layers.

### 3. Open-Loop Actuation Control
- **Problem:** Open-loop control of the pump and solenoid valve leads to inconsistent inflation rates and imprecise deflation speed, affecting measurement accuracy.
- **Status:** Identified limitation — closed-loop control is planned for the next phase.

---

## 🚀 Current Status

| Phase | Status |
|-------|--------|
| Schematic Design | ✅ Complete |
| PCB Layout (Altium) | ✅ Complete |
| Prototype Assembly | ✅ Functional |
| Oscillometric Algorithm | ✅ Basic Implementation |
| Closed-Loop Control | 🔄 In Progress |
| Noise Immunity Improvements | 🔄 In Progress |
| Measurement Accuracy Tuning | 🔄 In Progress |
| BLE / Mobile App | 📋 Planned |

---

## 📐 Oscillometric Method — Brief Explanation

1. Cuff is inflated above systolic pressure (artery fully occluded)
2. Cuff slowly deflates — blood starts pulsing through
3. HX710 captures micro-oscillations (amplitude variations) in cuff pressure
4. **Systolic pressure** → point where oscillation amplitude starts increasing
5. **Mean Arterial Pressure (MAP)** → point of maximum oscillation amplitude
6. **Diastolic pressure** → point where oscillation amplitude drops off

---

## 🛠️ Getting Started

### Prerequisites
- [ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/latest/) v5.x
- [Altium Designer](https://www.altium.com/) (for hardware files)
- Python 3.x (for build tools)

### Build & Flash Firmware
```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/bp-monitor.git
cd bp-monitor/firmware

# Set up ESP-IDF environment
. $IDF_PATH/export.sh

# Build
idf.py build

# Flash
idf.py -p /dev/ttyUSB0 flash monitor
```

---

## 🤝 Acknowledgements

Special thanks to **Eng. Elsayed Saeed** for the invaluable technical guidance and help in overcoming the hardware challenges faced during this project.

---

## 📬 Contact & Feedback

Open to technical challenges, feedback, and collaboration from embedded systems engineers.

Feel free to open an [Issue](../../issues) or reach out via [LinkedIn](https://www.linkedin.com/in/mahmoud-abdelaziz-554516222/).

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

*Tags: `#EmbeddedSystems` `#Mechatronics` `#PCBDesign` `#ESP32` `#BiomedicalEngineering` `#HardwareDesign` `#Altium` `#ControlSystems`*
