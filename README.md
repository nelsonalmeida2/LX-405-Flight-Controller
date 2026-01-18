<div align="center">

# 🚁 LX-405 Flight Controller

**A robust, Open Source FPV Flight Controller designed by a Computer Engineering student.**

[![Status](https://img.shields.io/badge/Status-Schematic%20Frozen-success?style=for-the-badge)](https://github.com/nelsonalmeida2/LX-405-Flight-Controller)
[![Version](https://img.shields.io/badge/Version-0.5-blue?style=for-the-badge)](https://github.com/nelsonalmeida2/LX-405-Flight-Controller)
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
Unlike typical hobbyist "flat" schematics, the LX-405 follows a strict **Hierarchical Design** approach verified by KiCad's Electrical Rules Check (**0 ERC Errors**).
Applying the software engineering principle of **Modularity** (Separation of Concerns), the hardware is divided into distinct hierarchical subsystems:
* **Power Management:** Encapsulates the voltage regulation logic (5V, 9V, 3.3V) with clean "Power Flags" logic.
* **MCU Core:** Contains the processor logic, clock, signaling, and boot configuration.
* **System Peripherals:** Groups active sensors (Gyro, Baro), OSD video processing logic, and user feedback systems (LEDs/Buzzer).
* **Connectors IO:** Isolates physical interface definitions (plugs/pads) from logical connections to prevent routing conflicts.

### 🎓 Academic Foundations
This design applies core concepts from my Computer Engineering curriculum at **Universidade Lusófona**:
* **Digital Systems:** Logic gates and boolean algebra applied to circuit switching.
* **Computer Architecture:** Understanding CPU data paths, buses, registers, and instruction execution.
* **Advanced Computer Architectures:** Knowledge of processor constraints, memory hierarchy (cache), and hardware-software interaction.
* **Fundamentals of Physics:** Electromagnetism, electric fields, circuit analysis, and DC power dynamics.

---

## ⚡ Technical Specifications

### 🧠 Core System (Schematic Completed)
* **MCU:** STM32F405RGT6 (168MHz, 1MB Flash).
* **Clock:** 8MHz HSE Crystal Oscillator with 22pF load caps for precision timing.
* **Signal Integrity:** Dedicated LC Filter (Ferrite Bead + Caps) for **VDDA** analog noise suppression.
* **Safety:** Hardware-implemented Watchdog (NRST) and Bootloader (BOOT0) physical controls.

### 📡 Connectivity & Protocols
* **USB Interface:** Modern **USB-C** connector with dedicated ESD protection lines for firmware updates and Betaflight configuration.
* **Motor Protocols:** 4x DSHOT Outputs (Timer-mapped for DMA collision avoidance) + ESC Telemetry (RX4).
* **Communication Ports:** 6x UARTs fully mapped for versatility:
    * **UART 1:** RC Link (ELRS / Crossfire).
    * **UART 2:** GPS / General Purpose.
    * **UART 3:** **Video Link** (SmartAudio for Analog **OR** MSP for DJI O3/HDZero).
    * **UART 4:** ESC Telemetry (Sensor Input).
    * **UART 5 & 6:** Peripheral Expansion (Bluetooth, Lidar, Secondary GPS).
* **Video System (Hybrid):**
    * Dedicated **"Hybrid Power Pad"** offering both **5V** (Analog) and **9V** (High Power Digital) options for the VTX.
    * **Analog OSD:** **MAX7456** (or **AT7456E**) on SPI2 with dedicated video filtering.
* **Sensor Bus:** Dedicated SPI1 (High-Speed Gyro) and I2C1 (Barometer).

### 🛡️ On-Board Sensors & Peripherals
* **IMU (Gyroscope):** **BMI270** connected via Low-Noise **SPI1**.
    * Features hardware **EXTI Interrupt** (PC4) for ultra-low latency loop synchronization.
* **Barometer:** **DPS310** connected via **I2C1** for high-precision altitude hold.
    * Hardware address selection implemented via SDO Pull-down logic.
* **User Feedback:**
    * **Status LEDs:** Dedicated **LED_POWER** (Red) and **LED_STATUS** (Blue) for instant system diagnostics.
    * **Buzzer Driver:** Integrated active buzzer circuit for alarms and "turtle mode" feedback.

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

### Phase 1: Schematic Design 🔌
- [x] **Power Management**
    - [x] Input Stage: TVS protection and Bulk Capacitance (3S-6S filtering).
    - [x] 5V Regulator: Synchronous Buck (TPS54302).
    - [x] 9V Regulator: High-current Buck (TPS54302) for HD Video.
    - [x] 3.3V Regulator: Low-Noise LDO (AP2112K) for sensors.

- [x] **MCU Core**
    - [x] STM32F405RGT6: Symbol integration and pin definition.
    - [x] Power Decoupling & Analog Filtering (VDDA).
    - [x] Clock System: 8MHz HSE Crystal.
    - [x] I/O Architecture: Full UART/SPI/I2C mapping validation.

- [x] **System Peripherals**
    - [x] Gyroscope (BMI270) via SPI1.
    - [x] Barometer (DPS310) via I2C with Address Fix.
    - [x] On-Screen Display (**MAX7456 / AT7456E**) via SPI2.
    - [x] **Visual/Audio:** Status LEDs & Buzzer Circuit implemented.

- [x] **Connectors & I/O**
    - [x] USB-C Interface (Data lines & ESD protection logic).
    - [x] ESC Connector (Standard pinout with Telemetry).
    - [x] **Hybrid VTX Connector** (Selectable 5V/9V).

- [x] **Design Validation**
    - [x] Electrical Rules Check (ERC): **PASSED with 0 errors**.
    - [x] Schematic Design Finalized.

### Phase 2: PCB Layout 📐
- [ ] **Physical Implementation**
    - [ ] Assign Footprints (Associating symbols with physical dimensions).
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


