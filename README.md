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

| Ref    | Component                              | Description                                                                                         | Qty | Unit Price            | Total        |
| ------ | -------------------------------------- | --------------------------------------------------------------------------------------------------- | --- | --------------------- | ------------ |
| RPI5   | Raspberry Pi 5/2GB                     | Raspberry Pi 5 Single Board Computer, 2GB RAM                                                       | 1   | \$65.00               | \$65.00      |
| TOF    | TOF400C VL53L1X                        | 4M Laser Ranging Sensor Module, TOF Time-of-Flight Distance, IIC Output, 4pcs                       | 1   | \$21.89               | \$21.89      |
| WIRE   | Fermerry 22AWG Stranded Wire           | 22AWG Stranded Wire Spool, 10ft Each, 6 Colors, Flexible Silicone Hook up Wire Kit, Tinned Copper   | 1   | \$12.29               | \$12.29      |
| GPS    | NEO-6M GPS Module                      | GPS Module Receiver, Navigation Satellite Positioning, with Antenna, High Sensitivity               | 1   | \$9.99                | \$9.99       |
| VEST   | JKSafety Hi Vis Reflective Safety Vest | Hi Vis Reflective Safety Vest with Pockets, Mesh Fabric, Construction Safety Apparel, Black, Size L | 1   | \$9.99                | \$9.99       |
| HAPTIC | HilBeliFU Vibration Motor Module       | DC5V 9000RPM Vibration Motor Module, PWM Control, for DIY/Wearable Smart Device, 3pcs               | 1   | \$7.65                | \$7.65       |
| CAM    | Arducam 5MP Camera for Raspberry Pi    | 5MP OV5647 Camera Module V1, 1080P HD, for Raspberry Pi 5/4/3/3B+                                   | 1   | \$6.99                | \$6.99       |
| PSU    | Raspberry Pi 5 Power Supply            | PD Adapter USB C, 5V 5A Power Adapter for Raspberry Pi 5                                            | 1   | \$11.99               | \$11.99      |
| CONN   | JST-XH 2.54mm Connector Kit            | 460Pcs JST-XH Connector Kit, 2.54mm Male/Female 2/3/4/5/6Pin Header Housing, Terminal Wires         | 1   | \$7.58                | \$7.58       |
|        |                                        |                                                                                                     |     | **Subtotal**          | **\$153.37** |
|        |                                        |                                                                                                     |     | **Shipping (Pishop)** | **\$11.51**  |
|        |                                        |                                                                                                     |     | **Shipping (Amazon)** | **\$0.00**   |
|        |                                        |                                                                                                     |     | **Tax (Pishop)**      | **\$3.45**   |
|        |                                        |                                                                                                     |     | **Tax (Amazon)**      | **\$4.69**   |
|        |                                        |                                                                                                     |     | **TOTAL**             | **\$173.02** |

### PCBWay

| Ref | Component | Description | Qty | Unit Price | Total |
|-----|-----------|-------------|-----|------------|-------|
| PCB | Custom Pi 5 HAT PCB | 2 layer, HASL finish, 1.6mm FR4 | 5 | $4.10 | $20.50 |

**Subtotal (PCBWay):** $FREE (grant)

### PCB Parts On Hand
All Pi Hat Connectors
**Subtotal (On Hand):** $0.00

### Cost Summary

| Category | Amount |
|----------|--------|
| Purchased Components | $173.02 |
| PCBWay (Custom HAT) | $00.00 |
| **Grand Total** | **$173.02** |

---

## Built With

- Raspberry Pi 5
- Python
- Time of Flight (ToF) distance sensors
- AI powered object detection
- Custom PCB HAT design
- Haptic motor drivers
- GPS positioning modules
