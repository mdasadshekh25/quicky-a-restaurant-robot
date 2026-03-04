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

---

## System Schematics & Wiring

The following diagrams illustrate the electrical connections between the ESP32 microcontroller and the peripheral modules.

### 1. System Schematic
This diagram shows the complete integration of the ESP32, Motor Driver, and Power Management system.

![System Schematic](https://github.com/user-attachments/assets/f2e35dd6-90a9-474c-9649-a687760f3d93)

### 2. Motor Driver (L298N) Connection
The L298N module is used to bridge the high-current DC motors with the ESP32 logic pins. It is powered directly from the Li-ion battery pack via the Buck Converter.

![Motor Driver Connection](https://github.com/user-attachments/assets/6b2a07df-6c8e-41b2-bf55-ee04bd26dedf)

### 3. Servo Lock Mechanism
The SG90 Servo motor handles the physical locking of the food compartment. It is connected to a PWM-capable GPIO on the ESP32.

![Servo Motor Connection](https://github.com/user-attachments/assets/f280daa1-1575-4085-80fb-222751d32e2b)

### 4. RFID (RC522) SPI Connection
The RC522 module communicates with the ESP32 via the SPI protocol. This diagram ensures the correct wiring of the Data (SDA), Clock (SCK), and Master/Slave (MOSI/MISO) lines.

![RFID Pin Connection](https://github.com/user-attachments/assets/9e3886e3-f0cd-435b-8ce5-837ea7b93c10)

---

![Project Wiring](https://github.com/user-attachments/assets/3b618d07-0318-430b-ba9f-15fb39d25ec9)

---

##  Software Design & Implementation

The software core logic connects all hardware components and enables the system’s functionality. The entire program was developed using a single environment and a set of specialized libraries.

###  Development Environment
* **Software:** The **Arduino IDE** (Integrated Development Environment) was chosen for its simplicity and extensive community support.
* **Board Support:** The **ESP32 core** for Arduino IDE was installed via the Board Manager to provide necessary compilers and tools.

###  Key Libraries Used
| Library | Purpose |
| :--- | :--- |
| `SPI.h` | Built-in library for Serial Peripheral Interface communication between ESP32 and RFID. |
| `MFRC522.h` | Main library used to initialize the RC522, detect cards, and read UIDs. |
| `WiFi.h` | Official library used to connect the ESP32 to the local Wi-Fi network as a client. |
| `WebServer.h` | Used to create the web server on Port 80 to handle HTTP requests from the operator. |

> **Note on Servo Control:** The `Servo.h` library was **not** used. Instead, the servo was controlled via the ESP32’s native **LEDC (PWM) functions** (`ledcAttach`, `ledcWrite`) for more precise hardware-level control.

###  Program Logic & Flow
The program handles two main tasks simultaneously using a non-blocking structure:

#### 1. Setup Function (`setup()`)
* Initializes the Serial monitor, SPI bus, and MFRC522 module.
* Attaches the servo and motor driver pins to the ESP32’s PWM controller.
* Connects to Wi-Fi and starts the WebServer to listen for movement commands.

#### 2. Main Loop (`loop()`)
* **Web Server:** Calls `server.handleClient()` to check for operator commands (Forward, Left, Stop, etc.).
* **RFID Check:** Continuously scans for a new card using `PICC_IsNewCardPresent()`.

###  RFID & Compartment Sequence
When a card is detected, the program follows this security logic:
1. **Read UID:** The program reads the unique ID of the scanned card.
2. **Validation:** It compares the UID to a hard-coded "master" UID (**82:B3:6E:05**).
3. **Action:** * **If Valid:** The continuous rotation servo spins for 1 second to toggle the lock (Open or Closed).
   * **If Invalid:** The code does nothing and the compartment remains locked.
4. **Halt:** Calls `PICC_HaltA()` to prevent reading the same card multiple times.

---

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

---


![Untitled design](https://github.com/user-attachments/assets/017dc98c-f702-4bee-9239-5ec0933162b9)
![1](https://github.com/user-attachments/assets/7351ad61-64df-4239-8002-bfe85002f3e5)
![IMG20251108183600](https://github.com/user-attachments/assets/b344517b-9555-4fa4-8154-7e47b184f50c)
![IMG20251108183549](https://github.com/user-attachments/assets/9bde45e8-bbc8-4bbe-bfe3-8c1d601c898a)
![IMG20251108183507](https://github.com/user-attachments/assets/7860032a-4479-4623-8eb0-047e8d1229fa)
![IMG20251108183409](https://github.com/user-attachments/assets/fbb4a7e5-ff97-4883-beab-4165b1268bd7)
![IMG20251108183356](https://github.com/user-attachments/assets/f77feedd-25be-4de0-a5f4-9341132988ef)

