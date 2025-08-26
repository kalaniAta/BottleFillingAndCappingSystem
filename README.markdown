# Bottle Filling and Capping Machine

## Overview

This project contains two Arduino sketches (`organized_bottle_filling.ino` and `organized_sensor_control.ino`) designed to control a bottle filling and capping machine using ESP32 microcontrollers. The system automates the process of filling bottles with a specified liquid volume and capping them, integrating various sensors, motors, and a keypad for user input.

- **organized_bottle_filling.ino**: Manages the actuation components, including stepper motors, DC motors, servos, a solenoid valve, and an LCD display for status updates.
- **organized_sensor_control.ino**: Handles sensor inputs (ultrasonic, IR, limit switches, and flow sensor) and keypad interactions, communicating with the actuator ESP32 to coordinate the filling and capping process.

## Hardware Requirements

- **ESP32 Microcontrollers** (2 units: one for actuation, one for sensors)
- **LiquidCrystal_I2C** LCD (16x4, address 0x27)
- **Keypad** (4x4, connected via PCF8574 I2C expander at address 0x20)
- **Sensors**:
  - Ultrasonic Sensors 
  - IR Sensors (2 units)
  - Limit Switches (5 units)
  - Flow Sensor (pulse-based, calibrated at 400 pulses per liter)
- **Actuators**:
  - Stepper Motors (4 units for positioning)
  - DC Motors (2 units for water pump and capping)
  - Servo Motors (2 SG90 and 1 MG995 for bottle clamping)
  - Solenoid Valve (controlled via relay)
- **Wiring Components**:
  - I2C communication (SDA: GPIO 21, SCL: GPIO 22)
  - Serial communication (RXD2: GPIO 16, TXD2: GPIO 17)

## Software Requirements

- **Arduino IDE** or compatible environment
- **Libraries**:
  - `Wire.h` (I2C communication)
  - `LiquidCrystal_I2C.h` (LCD control, ensure ESP32-compatible version)
  - `Keypad.h` and `Keypad_I2C.h` (keypad input)
- **ESP32 Board Support**: Install via Arduino Board Manager

## Setup Instructions

1. **Hardware Setup**:

   - Connect the sensors, actuators, and keypad to the respective ESP32 pins as defined in the pin definitions sections of both sketches.
   - Ensure proper I2C and serial communication wiring between the two ESP32s.
   - Verify the flow sensor calibration factor (default: 400 pulses per liter) matches your sensor.

2. **Software Setup**:

   - Clone this repository or download the `.ino` files.
   - Install the required libraries in the Arduino IDE.
   - Upload `organized_bottle_filling.ino` to the actuator ESP32 and `organized_sensor_control.ino` to the sensor ESP32.

3. **Operation**:

   - Power on both ESP32s. They will perform a handshake via serial communication.
   - Use the keypad to initiate a new order by pressing 'A'.
   - Select bottle size (1: 500ml, 2: 1050ml, 3: 1500ml) and quantity using the keypad.
   - The system will guide the user through the process via the LCD display, handling bottle positioning, filling, and capping.

## Features

- **Bottle Size Selection**: Supports 500ml, 1050ml, and 1500ml bottles with predefined servo angles and stepper motor steps.
- **Quantity Input**: Allows users to specify the number of bottles to process (up to 99).
- **Automated Process**:
  - Conveyor belt movement using stepper motors.
  - Precise liquid filling with flow sensor feedback.
  - Bottle clamping and capping with servo and DC motors.
  - Solenoid valve control for liquid dispensing.
- **Error Handling**:
  - Detects issues like missing bottles, caps, or flow errors.
  - Supports pause ('C') and stop ('D') commands via keypad.
- **Status Display**: Real-time updates on the LCD, including machine status, bottle count, and error messages.
- **Handshake Protocol**: Ensures synchronized operation between actuator and sensor ESP32s.

## Usage

- **Start**: Press 'A' on the keypad to begin a new order.
- **Pause/Resume**: Press 'C' to pause; press 'C' again to resume.
- **Stop**: Press 'D' to stop and reset the system.
- **Error Recovery**: The system pauses and displays errors (e.g., bottle not held, no caps) on the LCD, resuming after user intervention.

## Notes

- Ensure all sensors and actuators are calibrated and tested individually before full operation.
- The flow sensor calibration factor may need adjustment based on the specific sensor used.
- Serial communication (9600 baud) is critical for coordination between the two ESP32s; ensure reliable wiring.
- Commented code sections (e.g., alternative implementations) are preserved for reference but not used in the current logic.

## Troubleshooting

- **No Handshake**: Check serial connections (RXD2, TXD2) and ensure both ESP32s are powered and running.
- **LCD Issues**: Verify I2C address (0x27) and connections (SDA, SCL).
- **Sensor Errors**: Confirm sensor wiring and calibration; adjust `CALIBRATION_FACTOR`, `USMAX`, or `USMIN` if needed.
- **Motor Issues**: Check power supply and driver connections for steppers, DC motors, and servos.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contributing

Contributions are welcome! Please submit a pull request or open an issue for suggestions, bug reports, or improvements.