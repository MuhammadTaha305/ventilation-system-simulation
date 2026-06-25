# 🌬️ Smart Ventilation System Simulation

> 📚 **Course:** Computer Architecture — Fall 2025, NUST SEECS

The **Smart Corridor Ventilation & Air Quality Monitoring System** is a comprehensive exploration of IoT, low-level computer architecture, and 3D visualization — an automated system that monitors air safety and manages ventilation in a smart building. Built as a **Computer Architecture course project** (NUST SEECS).

This repository contains three distinct layers that model the full stack of the system, integrated over a shared **MQTT** message bus (HiveMQ public broker):
1.  **Hardware Layer (Wokwi)**: ESP32-based sensor node in C++ that reads sensors and publishes telemetry over MQTT.
2.  **Architecture Layer (Ripes)**: RISC-V assembly implementing the safety logic with memory-mapped I/O.
3.  **Visualization Layer (React/Three.js)**: A high-fidelity 3D "digital twin" that subscribes to the same MQTT topics and reacts in real time.

The three layers talk over MQTT topics under `smart-corridor/*` — the Wokwi node publishes sensor readings, the logic layer drives the safe/danger decision, and the 3D twin visualizes the resulting state and fan response live.

---

## 📂 Project Structure

- **`wokvi/`**: Contains the ESP32 firmware code (`sketch.ino`) for the Wokwi online simulator.
- **`ripes/`**: Contains the RISC-V assembly code (`ripes.s`) for the Ripes processor simulator.
- **`Simulation-room/`**: A modern React + Three.js web application providing an immersive 3D visualization of the environment.

---

## 🛠️ Components Overview

### 1. Wokwi Simulation (IoT Layer)
This module simulates the physical edge device responsible for gathering sensor data and communicating with the cloud.

- **Platform**: ESP32
- **Language**: C++ (Arduino Framework)
- **Key Features**:
    - **Sensors**: Reads analogs values from Potentiometers (simulating PM2.5 and CO2 levels) and digital values from a Switch (simulating Smoke).
    - **Actuators**: Controls LEDs to indicate status (Green=Safe, Red=Danger) and drives a Fan component.
    - **Connectivity**: Connects to WiFi and publishes real-time sensor data to a public MQTT broker (`broker.hivemq.com`) on topic `smart-corridor/monitor`.

**How to Run:**
1.  Open the `wokvi/sketch.ino` file.
2.  Copy the code into a project on [Wokwi.com](https://wokwi.com).
3.  Add an ESP32, Potentiometers, LEDs, and a Switch to the diagram.
4.  Start the simulation to see MQTT messages interacting with the broker.

### 2. Ripes Simulation (Architecture Layer)
This module demonstrates the low-level logic required to process sensor inputs and drive outputs directly at the processor level.

- **Platform**: RISC-V Processor Simulator (Ripes)
- **Language**: RISC-V Assembly
- **Key Features**:
    - **Memory Mapped I/O**: Reads sensor data from specific memory addresses (`0xf0000000`).
    - **Direct Hardware Control**: Manipulates an LED matrix mapped to `0xf0000008` to visualize air quality states (Green vs Red).
    - **Optimized Logic**: Efficient bitwise operations to mask and check individual sensor bits.

**How to Run:**
1.  Download and install [Ripes](https://github.com/mortbopet/Ripes).
2.  Open `ripes/ripes.s` in Ripes.
3.  Configure the "IOs" tab to match the memory addresses in the code:
    - **Switches (Bank 0)**: `0xf0000000` (Bits used for Smoke & PM2.5)
    - **Switches (Bank 1)**: `0xf0000004` (Bits used for CO2)
    - **LED Matrix**: `0xf0000008`
4.  Run the processor and toggle the switches to see the LED matrix react.

### 3. Simulation Room (Visual Layer)
A "Digital Twin" style 3D visualization that provides an intuitive user interface for the system.

- **Tech Stack**: React, Vite, Three.js (@react-three/fiber), Rapier (Physics).
- **Key Features**:
    - **Immersive 3D Environment**: Walk around a simulated house with a first-person controller.
    - **Dynamic Particles**: Visual fog simulation representing air pollution levels.
    - **Interactive Dashboard**: Real-time overlay showing CO2, PM2.5, and Air Quality Index (AQI).
    - **Automated Response**: Visually demonstrates the ventilation fan turning on when pollution thresholds are exceeded.

**How to Run:**
1.  Navigate to the directory:
    ```bash
    cd simulation-room
    ```
2.  Install dependencies:
    ```bash
    npm install
    ```
3.  Start the development server:
    ```bash
    npm run dev
    ```
4.  Open your browser to the local URL (usually `http://localhost:5173`) to explore the 3D room.

---

## 🎨 System Logic
All three components share the same core safety logic:
- **Safe State**: Green LED/UI. Fan is OFF.
- **Warning/Critical State**: Red LED/UI. Fan turns ON.
    - Triggered if **Smoke** is detected.
    - Triggered if **PM2.5** > 100.
    - Triggered if **CO2** > 1000 ppm.

---

## 👥 Authors
Built collaboratively for the Computer Architecture course at NUST SEECS by:
- **[Muhammad Taha](https://github.com/MuhammadTaha305)**
- **[Faizan Anwar](https://github.com/m-faizananwar)**

Original repository: [m-faizananwar/Ventilation-System-Simulation](https://github.com/m-faizananwar/Ventilation-System-Simulation).

## 📝 License
This project is open-source and intended for educational purposes.