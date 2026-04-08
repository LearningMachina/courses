# Motor Control

## Concept

DC motors spin at a speed proportional to the voltage applied to them. A motor driver is a chip that sits between the Jetson's low-current GPIO pins and the high-current motor wires, translating PWM signals into real power. For a tracked robot, differential drive — varying the speed of each side — produces all turning motions.

## Topics

- How DC motors and motor drivers work
- PWM: pulse-width modulation explained
- Differential drive: how tracked robots steer
- PID control: keeping movement smooth and accurate
- Writing a simple drive loop in Python

## Example

```
PWM: a square wave whose duty cycle sets the average voltage.
  100% duty ≈ full speed forward
   50% duty ≈ half speed
    0% duty ≈ stopped
```

```python
# Simple differential drive with RPi.GPIO-compatible PWM
import Jetson.GPIO as GPIO
import time

LEFT_PWM_PIN  = 32
RIGHT_PWM_PIN = 33
FREQ_HZ = 1000

GPIO.setmode(GPIO.BOARD)
GPIO.setup(LEFT_PWM_PIN,  GPIO.OUT)
GPIO.setup(RIGHT_PWM_PIN, GPIO.OUT)

left  = GPIO.PWM(LEFT_PWM_PIN,  FREQ_HZ)
right = GPIO.PWM(RIGHT_PWM_PIN, FREQ_HZ)
left.start(0)
right.start(0)

def drive(left_pct, right_pct):
    """Set motor speeds as percentages (0–100)."""
    left.ChangeDutyCycle(max(0, min(100, left_pct)))
    right.ChangeDutyCycle(max(0, min(100, right_pct)))

drive(70, 70)   # straight forward
time.sleep(2)
drive(70, 30)   # turn right
time.sleep(1)
drive(0, 0)     # stop

left.stop()
right.stop()
GPIO.cleanup()
```

```python
# Minimal PID controller
class PID:
    def __init__(self, kp, ki, kd):
        self.kp, self.ki, self.kd = kp, ki, kd
        self._integral = 0.0
        self._prev_error = 0.0

    def update(self, error, dt):
        self._integral += error * dt
        derivative = (error - self._prev_error) / dt if dt > 0 else 0
        self._prev_error = error
        return self.kp * error + self.ki * self._integral + self.kd * derivative
```

## Exercise

1. Wire your motor driver to the Jetson according to its datasheet.
2. Run the `drive()` example and confirm forward / turn / stop behaviour.
3. Implement a `go_straight(duration)` function that drives forward for the given number of seconds then stops.
4. Add a PID loop that keeps both wheels at the same speed using encoder feedback (or simulate it with random noise).

## Check

Explain to the robot:

- What is duty cycle, and how does 50% duty cycle at 1 kHz translate to average voltage?
- Why does a tracked robot turn right when the left motor is faster?
- What does each of the three PID terms (P, I, D) correct for?
