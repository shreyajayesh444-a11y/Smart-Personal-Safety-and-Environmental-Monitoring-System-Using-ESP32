# Smart Personal Safety and Environmental Monitoring System (ESP32)

A low-cost, multi-sensor embedded system that monitors **hazardous gases, temperature, humidity, and heart rate** in real time on a single ESP32 board, and raises prioritised alerts through LEDs, a buzzer, and a 16x2 LCD. It works fully offline, with no internet connection needed.

Developed as a team project in the Department of Electronics and Communication Engineering, Amrita Vishwa Vidyapeetham, Amritapuri. The full write-up is in [`docs/Smart_Personal_Safety_ESP32_Paper.pdf`](docs/Smart_Personal_Safety_ESP32_Paper.pdf).

![Hardware prototype](images/hardware-prototype.png)

## Why this project

Most safety monitors watch a single parameter, so they can miss situations where several risks happen together. This system combines **environmental** and **physiological** sensing and decides which risk matters most, so the most dangerous condition is always reported first.

## Features

- Multi-sensor monitoring: gas (MQ-135), temperature and humidity (DHT22), heart rate (MAX30102)
- **Priority-based decision algorithm:** gas leak > abnormal heart rate > high temperature > high humidity
- Gas sensor **calibration routine** to set a stable baseline in normal air
- Three alert channels: red/green LEDs, active buzzer, and live status text on a 16x2 I2C LCD
- Runs standalone on the ESP32, with basic error handling and timing control
- Validated through **Wokwi simulation** and a **breadboard hardware prototype**

## System architecture

The design has three layers: **sensing**, **processing**, and **alerting**.

![Block diagram](images/block-diagram.png)

## How it works

![Methodology flowchart](images/methodology-flowchart.png)

1. Initialise the ESP32 and all sensors, and calibrate the gas sensor baseline
2. Continuously read the MQ-135, DHT22, and MAX30102
3. Analyse the readings and check for abnormal conditions
4. If something is abnormal, apply the priority logic and trigger the matching alert (buzzer, red LED, LCD message)
5. If everything is normal, show the normal status (green LED, "SYSTEM NORMAL")
6. Repeat

### Alert behaviour

| Condition | LCD message | Red LED | Green LED | Buzzer |
|---|---|---|---|---|
| Normal | `SYSTEM NORMAL` | OFF | ON | OFF |
| Gas leakage detected | `GAS ALERT!` | ON | OFF | ON |
| Abnormal heart rate | `HEART ALERT!` | ON | OFF | ON |
| High temperature | `TEMP HIGH!` | ON | OFF | ON |
| High humidity | `HUMID CRITICAL` | ON | OFF | ON |

![Simulation output across conditions](images/output-table-and-simulation.png)

## Hardware components

| Component | Purpose |
|---|---|
| ESP32 DevKit V1 | Main controller; reads sensors and runs the alert logic |
| MQ-135 gas sensor | Detects harmful gases and air pollutants |
| DHT22 | Measures ambient temperature and humidity |
| MAX30102 | Measures heart rate (BPM) |
| 16x2 LCD with I2C module | Shows live status and alert messages |
| Active buzzer | Audible alert |
| Red and green LEDs (with 220 ohm resistors) | Visual alert and normal-status indication |
| Breadboard, jumper wires, USB cable | Prototyping, connections, power, and programming |

![Hardware components](images/hardware-components.png)

## Software

- Language: **Embedded C/C++** (Arduino framework)
- IDE: **Arduino IDE** for the ESP32
- Simulation: **Wokwi**

## Simulation

The system was simulated in Wokwi before building the hardware. Example: gas alert state.

![Wokwi gas alert](images/wokwi-gas-alert.png)

## Results

- The gas module detected changes in air quality and triggered the highest-priority alert
- The DHT22 readings were categorised into temperature and humidity alert levels
- The heart rate module raised an emergency alert for abnormal readings
- When several abnormal conditions occur together, the highest-priority one is reported
- Simulation and the hardware prototype confirmed correct sensor interfacing and alert generation

## Applications

Industrial safety monitoring, home gas leak detection, elderly health monitoring, healthcare monitoring, and indoor temperature and humidity monitoring.

## Future scope

- IoT cloud connectivity for remote monitoring
- Mobile app for real-time alerts
- Additional sensors and AI-based prediction for smarter decisions

## Repository structure

```
esp32-safety-monitor/
├── README.md
├── docs/
│   └── Smart_Personal_Safety_ESP32_Paper.pdf
├── images/
└── src/            <- add your Arduino sketch (.ino) here
```

## Team

- Shreya Jayesh
- Safa Sirajudeen
- Anagha S Kumar

Department of Electronics and Communication Engineering, Amrita Vishwa Vidyapeetham, Amritapuri Campus.
