# Improved Synchro Transmitter–Receiver System for Remote Angular-Position Measurement

An improved, solid-state synchro receiver system designed to replace legacy electromechanical synchro receivers (CX–TR pairs) using an active Scott-T transformer network and an AD2S1210 Resolver-to-Digital Converter (RDC).

---

## Technical Overview

- **Application:** Remote angular position measurement (radar/satellite antenna azimuth, ship gyro-compass repeaters, flight control surface indicators).
- **Architecture:** 
  $$\text{Synchro Transmitter (CX)} \xrightarrow{\text{S1, S2, S3}} \text{Scott-T Network} \xrightarrow{\sin, \cos} \text{AD2S1210 RDC} \xrightarrow{\text{SPI}} \text{Microcontroller (STM32/Arduino)}$$
- **Target Accuracy:** $\le 0.1^\circ$ (12-bit to 16-bit digital resolution).

---

## System Architecture

![System Architecture Diagram](hardware/architecture.png)

The system captures three-phase stator outputs ($S_1, S_2, S_3$) from the synchro transmitter, converts them into two-phase quadrature ($\sin\theta, \cos\theta$) signals via an active Scott-T network, digitizes the angle using a Type-II tracking loop (AD2S1210 RDC), and reads the high-precision angular position over SPI using an STM32 or Arduino microcontroller.

---

## Hardware Implementation

### Active Scott-T Converter & RDC Interface Circuit
![Scott-T Network Schematic](hardware/schematic.png)

### Key Circuit Subsystems
1. **Input Transient Protection:** TVS diodes and attenuation resistors scale high-voltage stator inputs down to 2V RMS.
2. **Active Scott-T Op-Amp Array:** Precision op-amp stages (AD712/AD713) convert 3-phase AC signals to 2-phase $90^\circ$ orthogonal sine and cosine channels.
3. **Tracking Resolver-to-Digital Converter (AD2S1210):** A Type-II tracking loop digitizes position data without phase lag and generates loss-of-signal (LOS) and loss-of-tracking (LOT) diagnostics.
4. **Reference Excitation:** Ratiometric tracking loop driven by a synchronized 400Hz/5kHz reference oscillator.

---

## Repository Structure

```text
synchro-rdc-system/
├── README.md
├── LICENSE
├── hardware/
│   ├── architecture.png           # High-level system architecture diagram
│   └── schematic.png              # Scott-T & RDC circuit schematic diagram
├── simulation/
│   ├── synchro_rdc_sim.m          # MATLAB simulation script
│   └── synchro_rdc_sim.py         # Python simulation script (NumPy + Matplotlib)
└── firmware/
    ├── arduino/
    │   └── read_rdc_ad2s1210.ino  # Arduino SPI code for AD2S1210 RDC
    └── stm32/
        └── main.c                 # STM32 HAL C code for AD2S1210 RDC
