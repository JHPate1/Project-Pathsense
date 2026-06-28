# PathSense

> Autonomous spatial awareness for the visually impaired. A light-weight, intelligent, vest-wearable vision for your environment.

---

# WHAT IT IS

PathSense is a wearable computer vision navigation vest (HOLY JUMBLE OF WORDS) intended to provide users with a more confidence-gaining awareness of their surroundings. With a powerful Raspberry Pi 5 in its core, and a combined network of precise distance sensors and tactile & audio feedback, this vest allows individuals with any level of vision to traverse indoor and outdoor spaces alike.

![Vest](https://cdn.discordapp.com/attachments/1354640349103915079/1520605992608595988/CAMERA_2.png?ex=6a41ce30&is=6a407cb0&hm=af58d03a860cf8f8a8969e4e9bd65c712284e9ba13b513f0842108eea87338b9&)

---

# HOW IT WORKS

Essentially, PathSense is a complex, sensor-fused computer vision system rather than having one single sensor that dictates what is occurring in an environment, it combines and merges data from multiple inputs, checking for consistency and using the interpreted data in a reliable way.

# THE SENSORS

For this system, we have established a network of:

- 4 Time-of-Flight (ToF) sensors: To continuously determine precise distances in 3 directions of the space around the user, to a range of 4 meters.

- an AI Camera: To assess higher level object detection of hazards, landmarks, obstacles and the immediate surrounding terrain.

- GPS Module: To provide general global location awareness, including basic route following and orientation in city blocks and street space awareness including side walk identification.

The raw output from these sensors is sent to the custom written Python backend which interprets things real time.

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

- Interface Consolidation: All GPIO pins are mapped to dedicated, keyed JST connector sockets. Every sensor, motor, and module plugs in cleanly and allows for easy repairs.
- Vibration Resistant Design: The connectors and layout are built to stay reliable even while you are walking, running (not recommended yet), or navigating bumpy terrain.
- Neat Form Factor: No loose wires, no breadboards, no cable spaghetti. Just a clean, integrated stack that fits comfortably inside the vest.

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
