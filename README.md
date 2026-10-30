# Improved Synchro Transmitter–Receiver System for Remote Angular-Position Measurement

An improved, solid-state synchro receiver system replacing legacy electromechanical synchro receivers (CX–TR pairs) using a Scott-T transformer network and an AD2S1210 Resolver-to-Digital Converter (RDC).

## Technical Overview

- **Application:** Remote angular position measurement (radar/satellite antenna azimuth, ship gyro-compass repeaters, flight surface indicators).
- **Architecture:** 
  $$\text{Synchro Transmitter (CX)} \xrightarrow{\text{S1, S2, S3}} \text{Scott-T Network} \xrightarrow{\sin, \cos} \text{AD2S1210 RDC} \xrightarrow{\text{SPI}} \text{Microcontroller (STM32/Arduino)}$$
- **Target Accuracy:** $\le 0.1^\circ$ (12-bit to 16-bit resolution).

## Architecture Block Diagram

```text
+----------------+      S1, S2, S3      +------------------+     sin, cos     +--------------------+
|  Synchro Tx    | -------------------> |  Scott-T Network | ---------------> | AD2S1210 RDC IC   |
| (Rotary Trans) |                      | (3-wire to 4-wire|                  | (Type-II Tracking) |
+----------------+                      +------------------+                  +--------------------+
        ^                                                                               |
        | 400Hz Ref                                                                  SPI| Interface
        +-------------------------------------------------------------------------------+
                                                                                        v
                                                                             +---------------------+
                                                                             | Microcontroller     |
                                                                             | (STM32 / Arduino)   |
                                                                             +---------------------+
