# The Robot's Hardware

## Concept

Your robot is built on the NVIDIA Jetson Orin Nano — a small computer with both a CPU (for general tasks) and a GPU (for fast parallel computation, especially AI). Understanding the physical components helps you wire things safely and know what is possible before you write a single line of code.

## Topics

- What is a Jetson Orin Nano? CPU vs GPU explained simply
- The LEGO-compatible case: how it works and what can be attached
- Ports and connectors: USB, GPIO, UART, I2C, SPI, camera
- The motor driver board: what it does and how to wire it
- Powering the robot: battery, voltage rails, always power off before wiring
- Your first interaction: turning an LED on from the command line

## Example

```python
# Blink an LED on GPIO pin 18 using the Jetson GPIO library
import Jetson.GPIO as GPIO
import time

GPIO.setmode(GPIO.BOARD)
GPIO.setup(18, GPIO.OUT, initial=GPIO.LOW)

try:
    while True:
        GPIO.output(18, GPIO.HIGH)
        time.sleep(0.5)
        GPIO.output(18, GPIO.LOW)
        time.sleep(0.5)
finally:
    GPIO.cleanup()
```

```
Jetson Orin Nano — key connectors
──────────────────────────────────
USB-A  ×4    → keyboard, mouse, USB drives
USB-C  ×1    → power input / data
CSI          → ribbon-cable camera (fast, low latency)
GPIO header  → 40-pin: power, ground, digital I/O, UART, I2C, SPI
```

## Exercise

1. With the robot **powered off**, locate the 40-pin GPIO header and identify pin 1 (marked with a triangle or square pad).
2. Using the pinout diagram, find a 3.3 V pin, a GND pin, and a GPIO pin.
3. Connect an LED (with a 330 Ω resistor in series) between a GPIO pin and GND.
4. Power the robot on and run the example script above (adjust the pin number if needed).
5. Confirm the LED blinks, then `Ctrl+C` to stop cleanly.

## Check

Explain to the robot:

- Why must you power off before wiring?
- What is the difference between I2C and SPI, and when would you choose one over the other?
- What does the motor driver board do that the Jetson's GPIO pins cannot do directly?
