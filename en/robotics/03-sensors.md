# Sensors

## Concept

Sensors are how the robot perceives the world. Each type measures something different — distance, orientation, wheel rotation — and each communicates over one of a few standard protocols. Raw sensor data is always noisy; filtering is not optional.

## Topics

- Types of sensors: distance (ultrasonic, IR, lidar), IMU, encoders
- Reading sensor data over I2C, UART, SPI
- Filtering noisy sensor data (moving average, Kalman basics)
- Reacting to the environment: obstacle avoidance

## Code

```python
# Read an HC-SR04 ultrasonic distance sensor
import Jetson.GPIO as GPIO
import time

TRIG = 16
ECHO = 18
GPIO.setmode(GPIO.BOARD)
GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)

def measure_distance_cm():
    GPIO.output(TRIG, True)
    time.sleep(0.00001)
    GPIO.output(TRIG, False)

    start = time.time()
    while GPIO.input(ECHO) == 0:
        start = time.time()
    while GPIO.input(ECHO) == 1:
        stop = time.time()

    elapsed = stop - start
    return (elapsed * 34300) / 2   # speed of sound / 2

# Moving average filter
from collections import deque

class MovingAverage:
    def __init__(self, window=5):
        self._buf = deque(maxlen=window)

    def update(self, value):
        self._buf.append(value)
        return sum(self._buf) / len(self._buf)

# Read an I2C IMU (e.g., MPU-6050 via smbus2)
import smbus2

bus = smbus2.SMBus(1)
MPU_ADDR = 0x68
bus.write_byte_data(MPU_ADDR, 0x6B, 0)   # wake up

def read_accel():
    def word(high, low):
        val = (high << 8) | low
        return val - 65536 if val >= 32768 else val

    data = bus.read_i2c_block_data(MPU_ADDR, 0x3B, 6)
    ax = word(data[0], data[1]) / 16384.0
    ay = word(data[2], data[3]) / 16384.0
    az = word(data[4], data[5]) / 16384.0
    return ax, ay, az
```

## Action

1. Wire an HC-SR04 and print distance readings in a loop.
2. Apply a 5-sample moving average and compare the raw vs filtered output.
3. Write an `avoid_obstacle(threshold_cm)` function that stops the robot when something is closer than `threshold_cm`.
4. *(Stretch)* Read accelerometer data from an I2C IMU and detect when the robot is tilted more than 15°.

## Reflection

After observing the robot's behavior, reflect on:

- What are the differences between I2C, SPI, and UART? When would you choose each?
- Why does a moving average introduce latency, and how do you choose the window size?
- What is the Kalman filter trying to do, conceptually, that a moving average cannot?

## Extension

Modify the sensor code to change how the robot perceives its environment:

1. Change the moving average window from 5 to 20 samples — the robot's reactions become smoother but slower. Try 2 — it becomes twitchy. Find the sweet spot.
2. Modify `avoid_obstacle()` to turn left when obstacles are on the right, and right when on the left (add a second sensor) — the robot now makes directional decisions.
3. Combine the ultrasonic sensor and IMU: if the robot tilts more than 15° while driving, it should stop — the robot now has a safety reflex. Physical feedback becomes a guardian.
