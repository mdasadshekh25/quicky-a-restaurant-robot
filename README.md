# Project Quicky: A Restaurant Service Robot
![Quicky](https://github.com/user-attachments/assets/80debef2-0290-49b4-b4b1-3aeee96affc1)
![IMG20251108183657](https://github.com/user-attachments/assets/a4b494fb-d386-4da5-b576-600c4368a838)

## Overview
Quicky is an manually wifi controlled waiter robot developed as an EEE student project to address efficiency and hygiene challenges in crowded food service environments. By replacing manual food delivery, it reduces human-to-human contact and streamlines operations in small, busy venues.

The system is built on the ESP32 platform, utilizing its dual-core processing and built-in WiFi to host a web-based control interface. A key highlight of the project is the integration of an RC522 RFID module, which creates a "secure locker" system—ensuring that only the person with the correct tag can access the delivery compartment.

Core Purpose

- Efficiency: Minimizes the time waiters spend walking back and forth in crowded spaces.

- Hygiene: Provides a contactless delivery method via a secured compartment.

- Stability: Uses a multi-wheel chassis designed to navigate typical cafe flooring without tipping.

---

## Actual Concept: The Service Workflow

The project implements an end-to-end automated ordering and delivery system. Below is the step-by-step concept of how a customer interacts with **Quicky**:

### 1. Ordering & Payment
* **Vending Interface:** The customer approaches a physical vending machine or accesses the digital menu here: **[Project Quicky Digital Menu](https://projectquicky.my.canva.site/)**.
* **Food Selection:** The user selects their desired items through the interface.
* **Digital Payment:** Payment is processed via a mobile banking system integration.
* **RFID Issuance:** Once the transaction is successful, the customer receives a unique **RFID Card**.

### 2. Preparation & Loading
* **Storage:** The prepared meal is placed into a temperature-controlled **demo locker/fridge** on the robot.
* **Security:** The locker remains electronically locked to ensure hygiene and order security.

### 3. Manual Navigation & Delivery
* **WiFi Control:** The shopkeeper uses the **ESP32 Web Interface** to manually navigate the robot through the cafe to the customer's table.

### 4. Secure Collection
* **Verification:** The customer taps their **RFID Card** on the robot's **RC522 reader**.
* **Unlocking:** The system verifies the card, and the food locker automatically opens for the customer to retrieve their meal.

---

## Hardware Components

| Product Name | Qty |
| :--- | :---: |
| **ESP32 Dev Board (CH340)** | 1 |
| **L298N Motor Driver** | 1 |
| **16 GA DC Motors** | 2 |
| **RC522 RFID Module** | 1 |
| **Servo Motor (SG90)** | 1 |
| **Li-ion Battery Pack** | 3 |
| **Robot Wheels** | 2 |
| **Ball Caster Wheel** | 1 |
| **Buck Converter (LM2596)** | 1 |
| **Main Power Switch** | 1 |
| **RFID Card/Tag** | 1 |
| **Solderless Breadboard** | 1 |
| **Jumper Wires** | Var. |

---

## Pin Configuration

This table shows how the hardware components are connected to the ESP32 GPIO pins.

### Motor Control (L298N)
| Function | Component Pin | ESP32 GPIO |
| :--- | :--- | :---: |
| Left Motor Speed | ENA | **GPIO 26** |
| Left Motor Dir 1 | IN1 | **GPIO 33** |
| Left Motor Dir 2 | IN2 | **GPIO 14** |
| Right Motor Speed | ENB | **GPIO 21** |
| Right Motor Dir 1 | IN3 | **GPIO 15** |
| Right Motor Dir 2 | IN4 | **GPIO 4** |

### RFID Access System (RC522)
| Function | Component Pin | ESP32 GPIO |
| :--- | :--- | :---: |
| RFID (SPI SS) | SDA | **GPIO 5** |
| RFID (SPI Clock) | SCK | **GPIO 18** |
| RFID (SPI MOSI) | MOSI | **GPIO 23** |
| RFID (SPI MISO) | MISO | **GPIO 19** |
| RFID (Reset) | RST | **GPIO 22** |

### Compartment Lock
| Function | Component Pin | ESP32 GPIO |
| :--- | :--- | :---: |
| Servo Lock | Signal | **GPIO 13** |

