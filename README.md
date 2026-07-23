# Thesis-Car: Bluetooth-Controlled Arduino Robot

A thesis project implementing a Bluetooth-controlled robot car with dual motor control and real-time speed monitoring via wheel encoders. This repository contains Arduino firmware for an autonomous/remote-controlled vehicle system.

## Overview

This project demonstrates embedded systems integration with:
- **Dual DC motor control** with individual direction control (forward/backward/turn)
- **Bluetooth wireless communication** for remote control commands
- **Dual wheel encoders** with interrupt-driven pulse counting for real-time RPM monitoring
- **Speed measurement** on both left and right wheels independently

## Features

- ✅ **Bluetooth Control** – Remote operation via serial communication (HC-05/HC-06 modules)
- ✅ **Dual Motor Control** – Independent left and right motor drive
- ✅ **Real-time Speed Monitoring** – RPM calculation from wheel encoders
- ✅ **Directional Movement** – Forward, backward, left turn, right turn, and stop
- ✅ **Interrupt-driven Pulse Counting** – Accurate speed measurement without blocking

## Hardware Requirements

### Microcontroller
- Arduino Uno (or compatible board)

### Motors & Power
- 2x DC Motors with gear reduction
- Motor driver module (L298N or similar) to handle 4 control pins
- Power supply (5-12V depending on motors)

### Sensors
- 2x Optical wheel encoders (20 pulses per revolution - adjustable)
- Encoder discs (pre-mounted or custom)

### Communication
- Bluetooth module (HC-05 or HC-06)
- UART/Serial connection at 9600 baud

### Wiring Connections

```
Motor Control Pins:
  LEFT_FORWARD   → PIN 9
  LEFT_REVERSE   → PIN 10
  RIGHT_FORWARD  → PIN 11
  RIGHT_REVERSE  → PIN 12

Encoder Pins (Interrupt):
  ENCODER_LEFT   → PIN 2  (INT0)
  ENCODER_RIGHT  → PIN 3  (INT1)

Bluetooth Module:
  RX → TX (PIN 1)
  TX → RX (PIN 0)
  GND → GND
  VCC → 5V (via voltage divider if needed)
```

## Project Structure

```
Thesis-car/
├── Car              Single encoder version (basic implementation)
├── Car2             Dual encoder version (improved with independent RPM tracking)
└── README.md        This file
```

### Car (Version 1)
Basic implementation with a single encoder for overall speed measurement.

**Features:**
- Mono encoder pulse counting
- Single RPM output
- Simpler setup with one interrupt pin

### Car2 (Version 2) ⭐ Recommended
Enhanced implementation with dual independent encoders for precision speed control.

**Improvements:**
- Separate encoder on each wheel
- Independent left/right RPM calculation
- Better control of motor speed differential
- Enhanced initialization message
- More detailed serial output

## Software Commands

The Bluetooth module receives single character commands:

| Command | Action |
|---------|--------|
| `1` | Move forward |
| `2` | Move backward/reverse |
| `3` | Turn right |
| `4` | Turn left |
| `5` | Stop all motors |

## Serial Communication

Connect via serial monitor at **9600 baud** to see real-time output:

```
Car initialized!
Command received: 1
Left RPM: 245 | Right RPM: 243
Left RPM: 248 | Right RPM: 245
...
```

## Getting Started

### 1. Hardware Assembly
- Connect motor driver to Arduino pins as specified above
- Mount wheel encoders on motor shafts or wheels
- Connect Bluetooth module to Arduino UART pins
- Wire power supply with appropriate voltage regulation

### 2. Calibration
Adjust the `pulse_per_turn` constant based on your encoder disc:
```cpp
unsigned int pulse_per_turn = 20; // Adjust based on your encoder disc slots
```
- Count the number of slots/patterns on your encoder disc
- Update this value for accurate RPM calculation

### 3. Upload Firmware
- Open Arduino IDE
- Select **Arduino Uno** from Tools → Board
- Load desired firmware (Car or Car2)
- Compile and upload to your microcontroller

### 4. Testing
- Open Serial Monitor (9600 baud)
- Send test commands via serial terminal or Bluetooth app
- Verify motor responses and RPM readings

## Differences: Car vs Car2

| Feature | Car | Car2 |
|---------|-----|------|
| Encoders | 1 | 2 |
| RPM Output | Single value | Left & Right independent |
| Interrupt Pins | 1 (PIN 2) | 2 (PIN 2 & 3) |
| Speed Accuracy | Basic | Higher precision |
| Control Loop | Simple | Better motor balancing |
| Recommended for | Prototyping | Production/thesis |

## Tuning & Optimization

### Motor Speed Control
To adjust overall speed, modify PWM values (requires motor driver with PWM support):
```cpp
// Add after digitalWrite commands
analogWrite(pin, 255);  // 0-255 PWM values
```

### Speed Measurement Interval
Modify the update frequency (currently 100ms):
```cpp
if (millis() - TIME >= 100) {  // Change 100 to desired milliseconds
```

### Encoder Sensitivity
If encoder readings are noisy:
- Check physical connection and soldering
- Verify encoder alignment and disc condition
- Add software debouncing if needed

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Motors not responding | Check Bluetooth connection, verify command codes, test serial output |
| RPM readings are zero | Confirm encoder disc alignment, check pin connections, verify pulse_per_turn value |
| Inconsistent motor speed | Calibrate encoder pulses, check motor voltage, verify motor driver connections |
| Bluetooth not connecting | Verify baud rate (9600), check TX/RX connections, confirm module power |
| Serial garbage output | Ensure correct baud rate (9600), check USB cable quality |

## Future Enhancements

- [ ] PID control loop for motor speed regulation
- [ ] Mobile app interface with real-time telemetry
- [ ] Obstacle detection with ultrasonic sensors
- [ ] Autonomous navigation with compass/gyro
- [ ] Power management and battery monitoring
- [ ] Web dashboard for monitoring

## Requirements

- Arduino IDE 1.8.x or higher
- Arduino board (Uno/Mega/etc)
- Libraries: Standard Arduino libraries only (no external dependencies)

## Author

- **maisha0055**
- **Project Type:** Thesis/Academic Project
- **Created:** January 2026

## License

This project is provided as-is for educational and thesis purposes.

## Support & Questions

For issues or questions:
1. Check the Troubleshooting section above
2. Verify hardware connections match the wiring diagram
3. Review encoder calibration steps
4. Test each component individually (motors, encoders, Bluetooth)

---

**Note:** This is a thesis project. Ensure all connections are properly insulated and tested before powering on the system. Disconnect power when making any hardware modifications.
