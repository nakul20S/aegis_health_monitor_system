# AEGIS — IoT Wearable Health Monitoring System for Industrial Worker Safety

> **Edge-intelligent wearable system for real-time worker health monitoring in extreme industrial environments.**  
> Delivered to **Delphi-TVS Technologies** for cold chamber evaluation. IEEE EPICS Grant applicant (IAS Track, 2026).

[![Live Dashboard](https://img.shields.io/badge/Live%20Dashboard-Online-brightgreen?style=flat-square)](https://sairam-innovathon-vitals.web.app/)
[![Product Website](https://img.shields.io/badge/Product%20Website-Aegis%20Solution-blue?style=flat-square)](https://nakul20s.github.io/Aegis-Solution/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)
[![Startup India](https://img.shields.io/badge/Registered-Startup%20India%20%2F%20MSME-orange?style=flat-square)]()
[![IEEE EPICS](https://img.shields.io/badge/Grant-IEEE%20EPICS%20IAS%20Track-blueviolet?style=flat-square)]()

---

## 🔗 Quick Links

| Resource | Link |
|---|---|
| 🌐 Product Website | [nakul20s.github.io/Aegis-Solution](https://nakul20s.github.io/Aegis-Solution/index.html) |
| 📊 Live Dashboard | [sairam-innovathon-vitals.web.app](https://sairam-innovathon-vitals.web.app/) |
| 🏭 Industry Partner | Delphi-TVS Technologies (Cold Chamber Evaluation) |
| 🏛️ Institution | Sri Sairam Engineering College, Chennai |
| 🔬 Incubation | Sri Sairam Techno Incubator Foundation |

---

## Overview

AEGIS is a three-tier wearable health monitoring system designed for industrial workers operating in sealed, hazardous, and extreme-temperature environments — specifically cold storage chambers, blast freezers, and confined industrial zones.

The system shifts safety from **reactive monitoring** to **proactive prevention** by continuously tracking vital health parameters, detecting anomalies at the edge, and triggering multi-level alerts — all without cloud or internet dependency.

A complete product version (wearable unit + internal hub + external hub + alert system) was **delivered to Delphi-TVS Technologies** for cold chamber evaluation and testing.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        AEGIS Architecture                        │
├──────────────────┬──────────────────────┬───────────────────────┤
│  Wearable Band   │   Internal Hub       │   External Hub        │
│  (On Worker)     │   (Inside Zone)      │   (Outside Zone)      │
│                  │                      │                        │
│  • ECG (AD8232)  │  • ESP32-C6          │  • ESP32-C6           │
│  • Temp sensor   │  • Edge processing   │  • Tower light alerts │
│  • IMU (ICM-     │  • Multi-wearable    │  • Buzzer siren       │
│    20948)        │    aggregation       │  • Web dashboard      │
│  • SOS button    │  • RS-485 wired out  │  • Supervisor UI      │
│  • ESP32-C6 MCU  │                      │                        │
│                  │   ESP-NOW Wireless   │   RS-485 Wired        │
│   [Wearable] ──────────────────────────────────────────────►    │
└──────────────────┴──────────────────────┴───────────────────────┘
```

**Communication layers:**
- **Wearable → Internal Hub:** ESP-NOW (2.4 GHz, low-latency, no router required)
- **Internal Hub → External Hub:** RS-485 wired (immune to EMI, metal shielding, Faraday cage effect)
- **External Hub → Dashboard:** Wi-Fi → Firebase → Web App

---

## Hardware Stack

| Component | Part | Function |
|---|---|---|
| MCU (all nodes) | ESP32-C6 | Wi-Fi 6, BLE, ESP-NOW, RISC-V core |
| ECG Frontend | AD8232 | 3-lead ECG signal acquisition |
| ECG Signal Conditioning | Sallen-Key 2nd-order LPF (~11× gain) | Reduces output impedance to <0.05Ω; boosts ECG from ~0.1Vpp → ~1.1Vpp |
| IMU | ICM-20948 (9-axis) | Fall detection, motion analysis, posture |
| Temperature | Digital temp sensor | Core body + ambient temp monitoring |
| Communication (wired) | RS-485 | Noise-immune inter-hub link |
| Communication (wireless) | ESP-NOW | Low-latency wearable → hub link |
| Alert Output | Tower light + buzzer | 3-state visual/audio alert (Green/Yellow/Red) |

---

## Repository Structure

```
aegis_health_monitor_system/
├── firmware/
│   ├── Armband ESP32 C6 code        # Wearable node firmware
│   ├── External ESP32 code          # External hub firmware
│   └── Internal hub ESP32 C6 code   # Internal hub firmware
│
├── prototype/
│   ├── Armband_Checking_code        # Hardware validation scripts
│   ├── ESP32 C6_Armband Code        # Armband standalone test
│   └── Receiver code (ESP32 DEV)    # Receiver test firmware
│
├── docs/
│   ├── system architecture.png      # Full system block diagram
│   ├── Armband components.png       # Wearable hardware layout
│   ├── ESP32 C6 img.png             # MCU reference
│   ├── PCB external hub.png         # External hub PCB
│   ├── PCB internal hub p1.png      # Internal hub PCB (front)
│   ├── PCB internal hub p2.png      # Internal hub PCB (back)
│   ├── Prototype img.png            # Physical prototype photo
│   ├── dashboard img.png            # Dashboard screenshot
│   ├── radar sensor img.png         # Sensor reference
│   ├── Medical certificate.png      # Medical-grade validation reference
│   └── RD proof.png                 # R&D proof documentation
│
├── website/                         # Product info website source
├── .gitignore
├── LICENSE
└── README.md
```

---

## ECG Signal Conditioning — Key Engineering Detail

The AD8232 ECG module has a high output impedance (~1 kΩ) that caused over 1V ADC droop when directly connected to the ESP32-C6 ADC pin.

**Fix:** Configured the AD8232's internal op-amp as a **Sallen-Key second-order low-pass filter** with ~11× gain:
- Output impedance reduced from ~1 kΩ → **< 0.05 Ω**
- ECG signal amplitude boosted from **~0.1 Vpp → ~1.1 Vpp**
- Clean signal with no ADC loading effect

This is a non-trivial hardware fix that directly impacts ECG signal quality and diagnostic reliability.

---

## ECG Lead Configuration

| Cable Color | Electrode Placement |
|---|---|
| Black | RA — Right Arm |
| Blue | LA — Left Arm |
| Red | RL — Right Leg (reference) |

---

## Alert States

| State | Indicator | Trigger Condition |
|---|---|---|
| 🟢 Green | Tower light ON | All vitals normal |
| 🟡 Yellow | Yellow light | Warning — temp drop, prolonged stillness |
| 🔴 Red + Siren | Red light + buzzer | Emergency — fall detected, critical ECG |

Alerts activate locally at the edge — **no internet required** for emergency response.

---

## Industry Deployment

A complete product version of AEGIS — comprising the wearable safety band, internal hub, external hub, and alert system — was **delivered to Delphi-TVS Technologies** for cold chamber evaluation and real-environment testing.

This deployment validated the system's performance in sub-zero industrial conditions and informed design decisions around hardware ruggedization, communication reliability, and alert response latency.

---

## Validated Performance

| Parameter | Result |
|---|---|
| Alert latency | < 200 ms (edge-triggered) |
| Cold environment operation | Validated at −20°C |
| Data transmission success | 100% over 8-hour shift cycles |
| ECG signal quality | Medical-grade, post Sallen-Key conditioning |
| Offline operation | Full functionality without internet |

---

## Grant & Recognition

- **IEEE EPICS Grant** — Applied under IAS Track, 2026 (pending decision)
- **Startup India / MSME** — Registered under Aegis Solution
- **Sri Sairam Techno Incubator Foundation** — Incubation partner
- **Live-In-Labs Program** — Sri Sairam Engineering College (Sem 6)
- **SDG Alignment** — Goals 3, 8, 9 (Health, Decent Work, Industry Innovation)

---

## Team

| Name | Role |
|---|---|
| **Nakul S** | Founder & Architecture Lead — system design, firmware, hardware |
| **Sanjay Babu S** | Documentation & Academic Coordination |
| **Sandeep Kumar P** | Documentation & Presentation Support |

**Faculty Supervisor:** Mr. K. Rajkumar, Associate Professor, EEE — Sri Sairam Engineering College  
**Technical Mentor:** Mr. Jayandan SA, Senior Scientist — Sri Sairam Techno Incubation Foundation

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

*AEGIS — Engineering safety, protecting lives.*
