# Quadcopter Flight Controller — Vertical Velocity Mode

A team-built quadcopter using a Teensy 4.0 and a vertical-velocity flight controller adapted from Carbon Aeronautics' Part XX project.

## Project overview

The controller reads inertial and barometric sensor data, estimates orientation and vertical motion, calculates control outputs, and mixes those outputs for four motors. The project also involved assembling and soldering the airframe and electronics, then troubleshooting a rightward lean observed during flight.

This is an adapted team project. The original controller was not written entirely by me; the contributions below describe the work I did with my team.

## Hardware

- **Microcontroller:** Teensy 4.0
- **Orientation sensor:** GY-521 module with MPU-6050
- **Battery:** Turnigy 2S, 1300 mAh
- **Frame:** Lower and upper quadcopter frame components from Carbon Aeronautics
- **Barometer:** The firmware communicates with a BMP280-compatible sensor at I²C address `0x76`; confirm the exact module installed on this build.
- **Receiver and ESCs:** Flysky FS-iA6B
- **Motors and propellers:** GEPRC GR1105 motors, clockwise propellers Gemfan 3018R, and Gemfan 3018 for counter clockwise.

The sketch configures receiver input on pin 14, motor outputs on pins 1–4, and additional sensing/control pins. Confirm all pin assignments, sensor orientation, and the board's supported output functions against the actual wiring before compiling or testing.

## What the firmware does

- Reads MPU-6050 accelerometer and gyroscope measurements over I²C.
- Estimates roll and pitch with a one-dimensional Kalman filter.
- Reads barometer data and estimates altitude and vertical velocity with a two-state Kalman filter.
- Uses angle, rate, and vertical-velocity PID calculations.
- Mixes the control outputs into four motor commands.
- Reads receiver channels and monitors battery voltage/current.

## Team and personal contributions

- Assembled the quadcopter and soldered hardware connections.
- Investigated a rightward lean observed during flight by checking both the hardware and firmware.
- Adjusted accelerometer offsets to compensate for sensor bias: `AccX - 0.05`, `AccY + 0.01`, and `AccZ - 0.11`.
- Updated the vertical-velocity Kalman-filter matrix calculations, including inverting `L` before calculating the Kalman gain `K = P * ~H * L`.

These changes were made during troubleshooting. Document the test conditions and results here if you have them; do not interpret the offsets alone as proof that the lean was fully corrected.

## Build

1. Install the Teensy board support package for the Arduino IDE.
2. Install the **PulsePosition** and **BasicLinearAlgebra** libraries.
3. Open `VelocityModeFlightController.ino`, select **Teensy 4.0**, and compile.
4. Verify the exact receiver, ESC, motor, and barometer models, wiring, sensor orientation, receiver channel order, and motor numbering.
5. Confirm battery-sensing scaling and all pin assignments for this build.

## Testing and safety

The team observed a rightward lean during flight and investigated both hardware and software. Add further test results here, including whether the adjustments resolved the issue and under what conditions.

Remove all propellers for bench testing. Check sensor readings, receiver failsafe behavior, motor order and direction, output limits, and throttle cutoff before any powered flight test. A successful compile or formatting pass is not evidence that the flight controller is safe to fly.

## Source and license

This project is adapted from [Carbon Aeronautics — Part XX: Flight Controller, Vertical Velocity Mode](https://github.com/CarbonAeronautics/Part-XX-FlightControllerVerticalVelocityMode). The original source header identifies Carbon Aeronautics' material as licensed under **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)**.

This adaptation credits the original project and is shared under the same license terms. It is not endorsed by Carbon Aeronautics. See the [CC BY-NC-SA 4.0 license](https://creativecommons.org/licenses/by-nc-sa/4.0/) and retain the original attribution and license notice with redistributed copies.
