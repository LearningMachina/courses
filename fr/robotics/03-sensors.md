# Capteurs

## Concept

Les capteurs sont la manière dont le robot perçoit le monde. Chaque type mesure quelque chose de différent — distance, orientation, rotation des roues — et chacun communique via l'un des quelques protocoles standards. Les données brutes des capteurs sont toujours bruitées ; le filtrage n'est pas optionnel.

## Thèmes

- Types de capteurs : distance (ultrasons, IR, lidar), IMU, encodeurs
- Lecture des données de capteurs via I2C, UART, SPI
- Filtrage des données bruitées (moyenne mobile, bases du filtre de Kalman)
- Réagir à l'environnement : évitement d'obstacles

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

1. Câblez un HC-SR04 et affichez les mesures de distance en boucle.
2. Appliquez une moyenne mobile sur 5 échantillons et comparez la sortie brute à la sortie filtrée.
3. Écrivez une fonction `avoid_obstacle(threshold_cm)` qui arrête le robot lorsqu'un objet est plus proche que `threshold_cm`.
4. *(Défi supplémentaire)* Lisez les données de l'accéléromètre depuis un IMU I2C et détectez quand le robot est incliné de plus de 15°.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Quelles sont les différences entre I2C, SPI et UART ? Quand choisiriez-vous chacun ?
- Pourquoi une moyenne mobile introduit-elle de la latence, et comment choisir la taille de la fenêtre ?
- Que cherche à faire le filtre de Kalman, conceptuellement, qu'une moyenne mobile ne peut pas faire ?

## Extension

Modifiez le code des capteurs pour changer la façon dont le robot perçoit son environnement :

1. Changez la fenêtre de la moyenne mobile de 5 à 20 échantillons — les réactions du robot deviennent plus fluides mais plus lentes. Essayez 2 — il devient nerveux. Trouvez le juste milieu.
2. Modifiez `avoid_obstacle()` pour tourner à gauche lorsque les obstacles sont à droite, et à droite lorsqu'ils sont à gauche (ajoutez un deuxième capteur) — le robot prend désormais des décisions directionnelles.
3. Combinez le capteur ultrasonique et l'IMU : si le robot s'incline de plus de 15° en roulant, il doit s'arrêter — le robot dispose maintenant d'un réflexe de sécurité. Le retour physique devient un gardien.
