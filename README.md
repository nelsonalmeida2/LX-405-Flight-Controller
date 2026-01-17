<div align="center">

# 🚁 LX-405 Flight Controller

**A robust, Open Source FPV Flight Controller designed by a Computer Engineering student.**

[![Status](https://img.shields.io/badge/Status-Schematic%20Design-blue?style=for-the-badge)](https://github.com/nelsonalmeida2/LX-405-Flight-Controller)
[![Version](https://img.shields.io/badge/Version-0.2-blue?style=for-the-badge)](https://github.com/nelsonalmeida2/LX-405-Flight-Controller)
[![Power](https://img.shields.io/badge/Input-3S--6S%20LiPo-red?style=for-the-badge)](https://github.com/nelsonalmeida2/LX-405-Flight-Controller)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

</div>

---

## 🎯 Motivation & Philosophy

> *"In the modern era, Artificial Intelligence combined with human effort makes the impossible, inevitable."*

The **LX-405** project was born from a realization: as a Computer Engineering student entering the high-speed world of FPV, I found that too many flight controllers on the market suffer from electrical noise, weak regulators, and fragility under 6S voltage spikes.

My goal is to prove that **modern engineering is boundary-less**. Even though my degree is not strictly focused on electronics, the foundational skills acquired in my academic path—combined with a self-taught mindset and the leverage of **Large Language Models (LLMs)** as advanced design assistants—allow for the creation of professional-grade hardware.

This project demonstrates that with the right theoretical foundations and modern AI tools, it is possible to design a Flight Controller that is **more robust, cleaner, and potentially more cost-effective** at scale.

### 📐 Design Strategy: Modular & Hierarchical
Unlike typical hobbyist "flat" schematics, the LX-405 follows a strict **Hierarchical Design** approach.
Applying the software engineering principle of **Modularity** (Separation of Concerns), the hardware is divided into distinct hierarchical subsystems:
* **Power Management:** Encapsulates the voltage regulation logic (5V, 9V, 3.3V) in nested sub-sheets.
* **MCU Core:** Contains the processor logic, clock, signaling, and boot configuration.
* **System Peripherals:** Groups active sensors (Gyro, Baro) and OSD video processing logic.
* **Connectors IO:** Isolates physical interface definitions (plugs/pads) from logical connections.

### 🎓 Academic Foundations
This design applies core concepts from my Computer Engineering curriculum at **Universidade Lusófona**:
* **Digital Systems:** Logic gates and boolean algebra applied to circuit switching.
* **Computer Architecture:** Understanding CPU data paths, buses, registers, and instruction execution.
* **Advanced Computer Architectures:** Knowledge of processor constraints, memory hierarchy (cache), and hardware-software interaction.
* **Fundamentals of Physics:** Electromagnetism, electric fields, circuit analysis, and DC power dynamics.
---

## ⚡ Technical Specifications

### 🧠 Core System (Completed)
* **MCU:** STM32F405RGT6 (168MHz, 1MB Flash)
* **Clock:** 8MHz HSE Crystal Oscillator with 22pF load caps for precision timing.
* **Signal Integrity:** Dedicated LC Filter (Ferrite Bead + Caps) for VDDA analog noise suppression.
* **Safety:** Hardware-implemented Watchdog (NRST) and Bootloader (BOOT0) physical controls.

### 📡 Connectivity & Protocols
* **Motor Protocols:** 4x DSHOT Outputs (Timer-mapped for DMA collision avoidance).
* **Communication Ports:** 4x UARTs configured for modern FPV needs:
    * **UART 1:** RC Link (ELRS / Crossfire).
    * **UART 3:** VTX Control (MSP for Digital / SmartAudio for Analog).
    * **UART 4:** Bluetooth / Telemetry.
    * **UART 6:** GPS & Navigation.
* **Sensor Bus:** Dedicated SPI1 (High-Speed Gyro) and SPI2 (OSD).

### 🛡️ On-Board Sensors & OSD
* **IMU (Gyroscope):** **BMI270** connected via Low-Noise **SPI1**.
    * Features hardware **EXTI Interrupt** (PC4) for ultra-low latency loop synchronization.
* **Barometer:** **DPS310** connected via **I2C1** for high-precision altitude hold.
* **Analog OSD:** **MAX7456** (or AT7456E) on **SPI2**.
    * Includes dedicated **Video Filter** and 27MHz Crystal for artifact-free image overlay.

### 🔋 Power Plant (Tri-Rail Architecture)
A hierarchical power distribution system designed to minimize noise coupling and maximize versatility:

| Rail | Regulator | Specs | Application |
| :--- | :--- | :--- | :--- |
| **Input** | XT60 Direct | **3S-6S (10V - 25.2V)** | Wide-range input for Cinematic (6S) & Freestyle (4S) |
| **9V Rail** | TPS54302 | 3A Buck | **Hybrid Video Ready:** Supports DJI O3/Vista & Analog VTX |
| **5V Rail** | TPS54302 | 3A Buck | GPS, ELRS Receiver, Buzzer, OSD Chip |
| **3.3V Rail** | AP2112K | 500mA LDO | **Ultra-Low Noise** for MCU, Gyro & Baro |

---

## 📂 Repository Structure

```text
LX-405-Flight-Controller/
├── 📂 Hardware/          # KiCad Project Files (Schematics, PCB & Symbols)
├── 📂 Schematic/         # PDF Exports for easy viewing
├── 📂 Docs/              # Datasheets and Engineering Notes
├── 📄 LICENSE            # MIT License details
└── 📄 README.md          # Documentation
```
## 🛠 Development Roadmap

- [x] **Power Management**
    - [x] Input Stage: TVS protection and Bulk Capacitance (3S-6S filtering).
    - [x] 5V Regulator: Synchronous Buck implementation (TPS54302) for logic/peripherals.
    - [x] 9V Regulator: High-current Buck implementation for HD Video Systems (DJI/Walksnail).
    - [x] 3.3V Regulator: Low-Noise LDO (AP2112K) configuration for sensitive sensors.

- [x] **MCU Core**
    - [x] STM32F405RGT6: Symbol integration and pin definition.
    - [x] Power Decoupling: VDD capacitor network validation.
    - [x] Analog Filtering: VDDA LC Filter (Ferrite + Caps) for precision ADC readings.
    - [x] Clock System: 8MHz HSE Crystal Oscillator circuit with load matching.
    - [x] Control Logic: Hardware BOOT0 and NRST (Reset) circuits.
    - [x] I/O Architecture: Hierarchical Label mapping for SPI, UART, I2C, and DSHOT.

- [x] **System Peripherals**
    - [x] Gyroscope (BMI270) implementation via SPI1 (featuring EXTI Interrupt).
    - [x] Barometer (DPS310) implementation via I2C.
    - [x] On-Screen Display (OSD): MAX7456 implementation via SPI2 (with video filtering & external crystal).

- [ ] **Connectors & I/O**
    - [ ] USB-C Interface (Data lines & ESD protection).
    - [ ] ESC Connector (JST-SH 8-pin) standard pinout definition.
    - [ ] Motor Output Pads (Alternative solder points).
    - [ ] Signal Breakout (UARTs, VTX, RX, GPS) layout.

- [ ] **PCB Layout**
    - [ ] Stackup definition (4-Layer).
    - [ ] Component placement & Routing.

---

## 📜 License

This project is licensed under the **MIT License** — see the `LICENSE` file for details.

You are free to use, modify, and distribute this hardware design for private or commercial use, provided you include the original copyright notice.

As an **Open Source Hardware** project, contributions and forks are welcome.

---

<div align="center">

**Designed in Lisbon, Portugal 🇵🇹**  
Powered by Engineering Fundamentals & AI

</div>

