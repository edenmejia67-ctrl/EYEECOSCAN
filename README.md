# EcoEye Scan - 3D Simulator

AI-powered waste recognition and automated segregation demo (ESP32-CAM, 4 servos, 4 bins).
Single file `index.html`: Three.js r128 from cdnjs, no build step.

## Run
Open `index.html`, or publish with GitHub Pages: Settings > Pages > Deploy from branch > `main` / root.

## How the simulation works
1. **Randomize** (or Auto mode) puts a random object on the scan area.
2. The ESP32-CAM view appears on the laptop screen with a bounding box, class and confidence (simulated AI, adjustable misclassification rate).
3. The matching servo opens one lid. A hand (sometimes a gauntlet) puts the item in when you press **Put in bin (hand)**. No press in 12 s logs a timeout.
4. The lid closes, the servo detaches, and the system waits for the next object.

Click any part in 3D to read what it does. Panel: stats (accuracy, segregation efficiency, processing time), bin counters with FULL lock, serial monitor, CSV export, fault tests (low battery, stuck servo), High/Low quality.

## Wiring
| ESP32-CAM | Goes to |
|---|---|
| GPIO12 / 13 / 14 / 15 | Servo 1 Wet / 2 Dry / 3 Metal / 4 Plastic signal |
| U0R / U0T / 5V / GND | TTL TX / RX / 5V / GND (Type-C to laptop) |
| GPIO0 | GND only while uploading |
| GPIO4 | Built-in flash LED |

Battery: 2x 3.7 V in parallel > boost converter (5 V) > servo + rail. 470 uF across servo 5 V and GND. All GND common.
Note: GPIO12 is a boot-strapping pin; the servo signal must not pull it HIGH at boot.

## Firmware
`firmware/ecoeye_servo/ecoeye_servo.ino` drives the servos from serial commands. Real AI runs on the laptop (video over Wi-Fi from the CameraWebServer example, commands over the TTL serial link).
