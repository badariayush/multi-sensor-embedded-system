# Multi-Sensor Embedded System (Sanitized Submission)

***DISCLAIMER:***  
***This project includes a full PCB layout and schematic design which cannot be shared publicly due to confidentiality constraints.
The system architecture, design decisions, and sensor principles described below are accurate but intentionally generalized and sanitized. I chose to submit this project because it represents one of the most technically meaningful systems I have worked on, particularly in sensor integration and measurement reliability.***
---

## Overview

This project involved designing and integrating a multi-sensor embedded system for real-time monitoring in a constrained environment. The system interfaces multiple sensors through SPI, I2C, and analog front-end pathways into a central compute unit (Raspberry Pi), enabling synchronized data acquisition and processing.

The design required careful consideration of signal integrity, sensor interfacing, and reliability under non-standard operating conditions.

The entire observation system will end up in a FFF Polymer 3D printer made to be tested in space and will be launched for testing in the coming years. It will be tested to advance in-space additive manufacturing. 

---

## System Architecture

*PNG File: High-level system architecture showing SPI, I2C, GPIO, and analog sensor pathways.*

---

## Component Spotlight: Strain Gauge + Wheatstone Bridge

One component I find particularly interesting is the strain gauge measurement system.

The strain gauge operates by changing its electrical resistance in response to mechanical deformation. Its resistance will increase when stretched and decrease when it is compressed. This change in resistance is extremely small, so it is measured using a Wheatstone bridge configuration.

A Wheatstone bridge consists of four resistors arranged in a diamond structure with two voltage divider nodes specified at the two middle corners. When the bridge is balanced, the voltage difference between the two midpoint nodes is zero. When the strain gauge experiences deformation, its resistance changes slightly and the difference in voltage at the two nodes becomes non-zero. The voltage is measured at both of the midpoints and is compared. This is used because it is easier to amplify and filter the differences in voltage is used rather than the resistance of the strain gauge itself.

This configuration allows the system to:
- Detect extremely small resistance changes  
- Convert mechanical strain into a voltage signal  
- Reject common-mode noise through differential measurement  

The output of the bridge is then amplified and digitized using an ADC, enabling precise measurement of strain.

---

## Ease-of-Use and Reliability Design Features

Several design decisions were made to improve usability, reliability, and manufacturability:

### 1. Layered PCB Power Distribution
The PCB was designed with dedicated layers for power and ground distribution. This simplified routing, reduced voltage drops, and improved overall signal stability, particularly for analog measurements.

### 2. Through-Hole Sensor Connections
Most external sensor connections were implemented using through-hole components. This provided:
- Strong mechanical attachment  
- Increased durability during handling and integration  
- More reliable connections compared to surface-only interfaces  

This was especially important given environmental constraints where traditional mounting methods (such as plastic cable clamps) could lead to risks such as outgassing which is especially important in space/microgravity environments.

### 3. Centralized Sensor Integration Board
All sensors were routed through a single integration board, reducing wiring complexity and making the system easier to assemble, debug, and maintain. All sensors were directly remade with all ICs into the board to reduce the troubleshooting and mounting space that would be added if each sensor was bought seperately.

### 4. Modular Bus Architecture (SPI / I2C Separation)
Sensors were grouped by communication protocol, allowing individual subsystems to be tested or modified without affecting the entire system. This significantly improved debugging efficiency. Since not all sensor data will be accessed constantly, the bus system combined with the Klipper software allows for the Rpi to select sensors and communicate with them individually when needed. 

### 5. Clear Signal Path Separation (Analog vs Digital)
Analog sensor pathways (such as the strain gauge) were intentionally separated from high-speed digital lines to reduce noise coupling and improve measurement accuracy.

---
