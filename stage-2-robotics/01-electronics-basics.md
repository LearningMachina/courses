# Electronics Basics

## Concept

Before you connect anything to the robot, you need a mental model of electricity: voltage is the pressure, current is the flow, and resistance limits the flow. Getting this wrong can destroy a GPIO pin or even the Jetson — but getting it right opens up a world of sensors and actuators.

## Topics

- Voltage, current, resistance — just enough to not break things
- Reading a datasheet: what the important numbers actually mean
- GPIO pins: digital output (on/off), digital input, pull-up/pull-down
- Breadboarding: prototyping circuits safely before soldering
- Common components: LEDs, buttons, resistors, capacitors
- Power safety: why you never connect 5 V where 3.3 V is expected

## Code

```
Ohm's Law:  V = I × R

LED circuit:
  3.3 V ──[330 Ω]──[LED]── GND

Current = 3.3 V / 330 Ω ≈ 10 mA   ✓ safe for GPIO
```

```python
# Reading a button with internal pull-up
import Jetson.GPIO as GPIO

BUTTON_PIN = 15
GPIO.setmode(GPIO.BOARD)
GPIO.setup(BUTTON_PIN, GPIO.IN, pull_up_down=GPIO.PUD_UP)

while True:
    if GPIO.input(BUTTON_PIN) == GPIO.LOW:   # pressed (pulled to GND)
        print("Button pressed!")
```

```
Datasheet key numbers to check:
  VCC / VDD  — supply voltage (must match your rail: 3.3 V or 5 V)
  VIH / VIL  — logic HIGH/LOW thresholds for inputs
  IOH / IOL  — max current sourced/sunk by GPIO
  Absolute Maximum Ratings — never exceed these
```

## Action

1. Calculate the correct resistor value for an LED (Vf = 2.0 V) connected to 3.3 V, targeting 10 mA.
2. Wire the LED with that resistor on a breadboard and test it with the GPIO script from Stage 0.
3. Add a button to the breadboard and write a script that turns the LED on while the button is held.
4. Find the Jetson Orin Nano datasheet and locate the GPIO maximum current specification.

## Reflection

After observing the robot's behavior, reflect on:

- What happens if you connect a 5 V sensor output directly to a 3.3 V Jetson GPIO input?
- What does a pull-up resistor do, and why is it needed for a button?
- Why is a capacitor useful near a motor driver's power input?

## Extension

Modify the circuit to change the robot's physical signals:

1. Swap the 330 Ω resistor for a 1 kΩ resistor — the LED dims. The robot's signal is now quieter. Try 100 Ω (with caution) — it gets louder.
2. Add a second LED of a different colour on another GPIO pin and make them signal different states (green = safe, red = obstacle detected) — the robot now communicates with colour.
3. Wire a button and modify the code so the LED only lights when pressed — the robot now responds to human touch. This is your first input → output loop.
