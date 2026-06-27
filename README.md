# PathSense
> Autonomous spatial awareness for the visually impaired, a lightweight, intelligent wearable vest that sees the world and guides you through it.
---

## What It Is

PathSense is a wearable navigation system designed to give people with mild to severe visual impairments a more confident understanding of their surroundings. Built around a Raspberry Pi 5, the vest combines real time computer vision, precise distance sensing, and intuitive haptic audio feedback to help users move safely through both indoor and outdoor environments.

Whether you are navigating a crowded sidewalk, finding your way through a building, or just taking a walk, PathSense quietly works in the background, mapping obstacles, identifying hazards, and nudging you in the right direction through gentle vibrations and clear voice alerts.

---

## How It Works

At its core, PathSense is a sensor fusion platform. It does not rely on any single input to understand the world. Instead, it blends data from multiple sources, cross checks them for accuracy, and translates the result into feedback you can actually feel and hear.

### The Sensor Network

PathSense uses a hybrid layout of sensors to build a reliable picture of the space around you:
| Sensor | Role | Coverage |
|--------|------|----------|
| **4x Time of Flight (ToF) Sensors** | Real time distance mapping | Left, right, and center paths up to 4 meters |
| **AI Camera** | Contextual object detection | Obstacles, landmarks, hazards, and terrain |
| **GPS Module** | Ground level navigation | City coordinates, route context, sidewalk boundaries |

These sensors feed into a custom Python core that processes everything in real time and decides what you need to know, and when.

### The Feedback Loop

Once the system understands the environment, it communicates back to you through two channels:
- **Haptic Feedback**, Three vibration lines built into the vest deliver directional cues. A gentle pulse on your left shoulder means "obstacle on the left." Variable intensity tells you how close something is.
- **Audio Alerts**, For high priority information, a voice engine speaks concise alerts like "Person ahead" or "Stairs, stop." The audio layer is designed to be informative without being overwhelming.
---
## System Architecture
```
+-------------------------------------+
|    Raspberry Pi 5                   |
+--------------+----------------------+
               |
    +----------+----------+
    |          |          |
+---v---+  +---v---+  +---v---+
| 4x ToF|  |AI Cam |  |  GPS  |
|Sensor |  | Feed  |  |Module |
+---+---+  +---+---+  +---+---+
    |          |          |
    +----------+----------+
               |
    +----------v----------+
    |  Custom Python Core |
    |  (Sensor Fusion &   |
    |   Decision Logic)   |
    +----------+----------+
               |
    +----------+----------+
    |          |          |
+---v---+            +---v---+
|3x     |            |Audio  |
|Haptic |            |Engine |
|Lines  |            |       |
+-------+            +-------+
```
---
## Core Subsystems

### Spatial Distance Mapping
Four ToF sensors continuously monitor the environment for physical boundaries. They track distances to nearby walls, furniture, curbs, and other obstacles, giving the system a raw but reliable depth map of what is directly around you.

### Contextual Computer Vision
The camera feed runs an object detection model (YOLO) that adds meaning to the raw distance data. It can tell the difference between a person and a pole, identify an upcoming staircase, or flag uneven terrain. This contextual layer is what turns numbers into useful information.

### Dynamic Ground Navigation
When you are outdoors, the GPS module works alongside the camera to evaluate ground layouts, identify sidewalk edges, and avoid rocky or uneven paths.

### Dual Channel Feedback
Spatial data is translated into vibrations across three haptic motors. The intensity and location of each pulse tell you where to go and what to avoid. When something needs your immediate attention, like a sudden drop or an oncoming cyclist, the audio engine cuts in with a clear and sharp voice alert along with the activation of all haptic motors.

---



## Smart Sensor Arbitration

Wearable sensor arrays have a classic problem: false positives caused by normal human movement. Walking swings your arms. Eating brings your hands near your chest. Without intelligence, these motions would trigger constant, useless alerts. PathSense solves this by treating the camera as the primary source of truth. When you move your arms into the sensor path, the object detection model recognizes your hands in real time. The Python core then automatically suppresses data from any ToF sensors that are currently blocked. The result: no false positives.

---

## Custom Hardware: The Pi 5 HAT

PathSense uses a custom designed Raspberry Pi 5 expansion HAT to keep the wearable robust and wearable friendly.

- **Interface Consolidation**, All GPIO pins are mapped to dedicated, keyed JST connector sockets. Every sensor, motor, and module plugs in cleanly and allows for easy repairs.

- **Vibration Resistant Design**, The connectors and layout are built to stay reliable even while you are walking, running(not recommended yet), or navigating bumpy terrain.

- **Neat Form Factor**, No loose wires, no breadboards, no cable spaghetti. Just a clean, integrated stack that fits comfortably inside the vest.

---

## Bill of Materials (BOM)

### Purchased Components

| Ref | Component | Description | Qty | Unit Price | Total |
|-----|-----------|-------------|-----|------------|-------|
| RPI5 | iRasptek Starter Kit for Raspberry Pi 5 | 8GB RAM, preloaded 64GB Pi OS, red and white case | 1 | $224.99 | $224.99 |
| USB C | 50FT USB C Power Cable for CCTV Camera | Long flat USB C extension charging cable | 1 | $16.99 | $16.99 |
| HOTPLATE | WEP 946D IV 100x50mm Small LED Soldering Hot Plate | SMD/BGA rework reflow, 210W preheater with digital temp control | 1 | $35.99 | $35.99 |
| VEST | JKSafety Hi Vis Reflective Safety Vest | High visibility mesh vest with pockets, black, size L | 1 | $9.99 | $9.99 |
| CAM | innomaker Raspberry Pi Camera Module 5MP 1080P | OV5647 sensor with M12 FOV90 IR filter lens | 1 | $16.99 | $16.99 |
| HAPTIC | HilBeliFU Vibration Motor Module | DC5V 9000RPM, PWM control, 3 pack | 1 | $7.99 | $7.99 |
| GPS | DWEII GPS+BDS Dual Mode Module | ATGM336H flight control satellite positioning, 2 pack | 1 | $17.99 | $17.99 |
| TOF | TOF400C VL53L1X 4M Laser Ranging Sensor | Time of flight distance sensor, IIC output, 4 pack | 1 | $21.89 | $21.89 |
| OTG | USB 3.0 to USB C 3.1 Adapter | 10Gbps bi directional OTG converter, 2 pack | 1 | $6.99 | $6.99 |

**Subtotal (Purchased):** $358.48

### PCBWay

| Ref | Component | Description | Qty | Unit Price | Total |
|-----|-----------|-------------|-----|------------|-------|
| PCB | Custom Pi 5 HAT PCB | 2 layer, HASL finish, 1.6mm FR4 | 5 | $4.10 | $20.50 |


**Subtotal (PCBWay):** $20.50

### PCB Parts On Hand
All Pi Hat Connectors
**Subtotal (On Hand):** $0.00

### Cost Summary
| Category | Amount |
|----------|--------|
| Purchased Components | $358.48 |
| Shipping & Handling | $0.00 |
| PCBWay (Custom HAT) | $20.50 |
| **Subtotal** | **$378.98** |
| Estimated Tax | $21.00 |
| **Grand Total** | **$399.98** |

---
## Built With

- Raspberry Pi 5
- Python
- Time of Flight (ToF) distance sensors
- AI powered object detection
- Custom PCB HAT design
- Haptic motor drivers
- GPS positioning modules
---
*PathSense is designed to make independent navigation feel less like a challenge and more like second nature.* 
