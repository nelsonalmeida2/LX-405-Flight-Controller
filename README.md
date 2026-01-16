\# LX-405 Flight Controller 🚁

> \*\*A robust, Open Source FPV Flight Controller designed by a Computer Engineering student.\*\*



!\[Status](https://img.shields.io/badge/Status-In%20Development-yellow) !\[Version](https://img.shields.io/badge/Version-0.1-blue) !\[Power](https://img.shields.io/badge/Input-6S%20LiPo-red)



\## 🎯 Motivation \& Philosophy

The \*\*LX-405\*\* project was born from a realization: as a Computer Engineering student entering the high-speed world of FPV, I found that software performance is strictly limited by hardware reliability. Too many flight controllers on the market suffer from electrical noise, weak regulators, and fragility under 6S voltage spikes.



My goal is to prove that \*\*modern engineering is boundary-less\*\*. Even though my degree is not strictly focused on electronics, the foundational skills acquired in my academic path—combined with a self-taught mindset and the leverage of \*\*Large Language Models (LLMs)\*\* as advanced design assistants—allow for the creation of professional-grade hardware.



This project demonstrates that with the right theoretical foundations and modern AI tools, it is possible to design a Flight Controller that is \*\*more robust, cleaner, and potentially more cost-effective\*\* at scale than current market solutions.



\### 📐 Design Strategy: Modular \& Hierarchical

Unlike typical hobbyist "flat" schematics, the LX-405 follows a strict \*\*Hierarchical Design\*\* approach.

Applying the software engineering principle of \*\*Modularity\*\* (Separation of Concerns), the hardware is divided into logical functional blocks (Power Management, MCU Core, Connectors).

\* \*\*Abstraction:\*\* Complex sub-circuits (like voltage regulators) are encapsulated in their own sub-sheets, keeping the main logic clean and readable.

\* \*\*Debuggability:\*\* Issues can be isolated to specific modules without affecting the entire system.

\* \*\*Scalability:\*\* Modules can be upgraded or replaced individually (e.g., swapping the 5V regulator design) without redesigning the whole board.



\### 🎓 Academic Foundations

This design applies core concepts from my Computer Engineering curriculum at Universidade Lusófona:

\* \*\*Digital Systems:\*\* Logic gates and boolean algebra applied to circuit switching.

\* \*\*Computer Architecture:\*\* Understanding CPU data paths, buses, and memory hierarchies.

\* \*\*Advanced Computer Architectures:\*\* Knowledge of processor constraints, cache coherence, and hardware-software interaction.

\* \*\*Fundamentals of Physics:\*\* Electromagnetism, circuit analysis, and direct current (DC) power dynamics.



\## ⚡ Technical Specifications



\### 🧠 Core System

\* \*\*MCU:\*\* STM32F405RGT6 (168MHz, 1MB Flash)

\* \*\*IMU (Gyro):\*\* (TBD, e.g., BMI270 / MPU6000)

\* \*\*Firmware Target:\*\* Betaflight / INAV ready



\### 🔋 Power Plant (Tri-Rail Architecture)

A hierarchical power distribution system designed to minimize noise coupling:



| Rail | Regulator | Specs | Application |

| :--- | :--- | :--- | :--- |

| \*\*Input\*\* | XT60 Direct | 6S (25.2V) | Direct power for ESCs \& Sensing |

| \*\*9V Rail\*\* | TPS54302 | 3A Buck | DJI O3 Unit / Analog VTX / Camera |

| \*\*5V Rail\*\* | TPS54302 | 3A Buck | GPS, ELRS/TBS Receiver, Buzzer, LEDs |

| \*\*3.3V Rail\*\* | AP2112K | 500mA LDO | \*\*Ultra-Low Noise\*\* for MCU \& Gyro |



\*> \*\*Engineering Note:\*\* The 3.3V LDO is cascaded from the 5V rail (not VBAT) to maximize thermal efficiency and provide a double-filtration stage for sensitive sensors.\*



\## 📂 Repository Structure

\* `Schematic/` - PDF exports of the electrical schematics.

\* `Hardware/` - KiCad source files (.kicad\_sch, .kicad\_pcb).

\* `Docs/` - Datasheets and design notes.

\* `Production/` - Gerbers and BOM files.



\## 🛠 Development Roadmap

\- \[x] \*\*Power Management:\*\* 5V/9V/3.3V regulation stages validated.

\- \[ ] \*\*MCU Core:\*\* STM32F405 implementation \& Decoupling.

\- \[ ] \*\*Sensors:\*\* Gyroscope \& Barometer integration.

\- \[ ] \*\*Connectivity:\*\* UART ports, USB-C, and Motor outputs.

\- \[ ] \*\*PCB Layout:\*\* 4-Layer routing.



---

\*Designed in Lisbon, Portugal 🇵🇹 | Powered by Engineering Fundamentals \& AI\*

