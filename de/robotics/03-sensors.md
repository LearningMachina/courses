# Sensoren

## Konzept

Sensoren sind die Art, wie der Roboter die Welt wahrnimmt. Jeder Typ misst etwas anderes — Entfernung, Orientierung, Raddrehung — und jeder kommuniziert über eines von wenigen Standardprotokollen. Rohe Sensordaten sind immer verrauscht; Filtern ist nicht optional.

## Themen

- Sensortypen: Entfernung (Ultraschall, IR, Lidar), IMU, Encoder
- Sensordaten über I2C, UART, SPI lesen
- Verrauschte Sensordaten filtern (gleitender Mittelwert, Kalman-Grundlagen)
- Auf die Umgebung reagieren: Hindernisvermeidung

## Code

```python
# HC-SR04 Ultraschall-Entfernungssensor auslesen
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
    return (elapsed * 34300) / 2   # Schallgeschwindigkeit / 2

# Gleitender-Mittelwert-Filter
from collections import deque

class MovingAverage:
    def __init__(self, window=5):
        self._buf = deque(maxlen=window)

    def update(self, value):
        self._buf.append(value)
        return sum(self._buf) / len(self._buf)

# I2C-IMU auslesen (z. B. MPU-6050 über smbus2)
import smbus2

bus = smbus2.SMBus(1)
MPU_ADDR = 0x68
bus.write_byte_data(MPU_ADDR, 0x6B, 0)   # aufwecken

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

## Aktion

1. Verkable einen HC-SR04 und gib Entfernungsmesswerte in einer Schleife aus.
2. Wende einen 5-Sample-gleitenden-Mittelwert an und vergleiche die rohe vs gefilterte Ausgabe.
3. Schreibe eine Funktion `avoid_obstacle(threshold_cm)`, die den Roboter stoppt, wenn etwas näher als `threshold_cm` ist.
4. *(Zusatz)* Lese Beschleunigungsdaten von einer I2C-IMU und erkenne, wenn der Roboter mehr als 15° geneigt ist.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was sind die Unterschiede zwischen I2C, SPI und UART? Wann würdest du jeweils welches wählen?
- Warum führt ein gleitender Mittelwert zu Latenz, und wie wählt man die Fenstergröße?
- Was versucht der Kalman-Filter konzeptionell zu leisten, was ein gleitender Mittelwert nicht kann?

## Erweiterung

Verändere den Sensorcode, um zu ändern, wie der Roboter seine Umgebung wahrnimmt:

1. Ändere das Fenster des gleitenden Mittelwerts von 5 auf 20 Samples — die Reaktionen des Roboters werden glatter, aber langsamer. Versuche 2 — er wird zappelig. Finde den Sweet Spot.
2. Ändere `avoid_obstacle()`, sodass nach links ausgewichen wird, wenn Hindernisse rechts sind, und nach rechts, wenn links (füge einen zweiten Sensor hinzu) — der Roboter trifft jetzt Richtungsentscheidungen.
3. Kombiniere Ultraschallsensor und IMU: Wenn der Roboter sich während der Fahrt mehr als 15° neigt, soll er stoppen — der Roboter hat jetzt einen Sicherheitsreflex. Physisches Feedback wird zum Wächter.
