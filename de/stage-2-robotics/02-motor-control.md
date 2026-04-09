# Motorsteuerung

## Konzept

Gleichstrommotoren drehen sich mit einer Geschwindigkeit proportional zur angelegten Spannung. Ein Motortreiber ist ein Chip, der zwischen den stromschwachen GPIO-Pins des Jetson und den stromstarken Motorkabeln sitzt und PWM-Signale in echte Leistung übersetzt. Bei einem Kettenroboter erzeugt Differentialantrieb — die Geschwindigkeit jeder Seite variieren — alle Lenkbewegungen.

## Themen

- Wie Gleichstrommotoren und Motortreiber funktionieren
- PWM: Pulsweitenmodulation erklärt
- Differentialantrieb: Wie Kettenfahrzeuge lenken
- PID-Regelung: Bewegung glatt und genau halten
- Eine einfache Fahrschleife in Python schreiben

## Code

```
PWM: eine Rechteckwelle, deren Tastverhältnis die Durchschnittsspannung bestimmt.
  100% Tastverhältnis ≈ volle Geschwindigkeit vorwärts
   50% Tastverhältnis ≈ halbe Geschwindigkeit
    0% Tastverhältnis ≈ gestoppt
```

```python
# Einfacher Differentialantrieb mit RPi.GPIO-kompatibler PWM
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
    """Motorgeschwindigkeiten als Prozentwerte setzen (0–100)."""
    left.ChangeDutyCycle(max(0, min(100, left_pct)))
    right.ChangeDutyCycle(max(0, min(100, right_pct)))

drive(70, 70)   # geradeaus vorwärts
time.sleep(2)
drive(70, 30)   # rechts abbiegen
time.sleep(1)
drive(0, 0)     # stoppen

left.stop()
right.stop()
GPIO.cleanup()
```

```python
# Minimaler PID-Regler
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

## Aktion

1. Verkable deinen Motortreiber gemäß seinem Datenblatt mit dem Jetson.
2. Starte das `drive()`-Beispiel und bestätige das Vorwärts-/Abbiege-/Stopp-Verhalten.
3. Implementiere eine Funktion `go_straight(duration)`, die für die angegebene Sekundenzahl geradeaus fährt und dann stoppt.
4. Füge eine PID-Schleife hinzu, die beide Räder mit Encoder-Feedback auf der gleichen Geschwindigkeit hält (oder simuliere es mit zufälligem Rauschen).

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist das Tastverhältnis, und wie übersetzt sich 50 % Tastverhältnis bei 1 kHz in Durchschnittsspannung?
- Warum biegt ein Kettenroboter nach rechts ab, wenn der linke Motor schneller ist?
- Wofür korrigiert jeder der drei PID-Terme (P, I, D)?

## Erweiterung

Verändere den Motorcode, um zu ändern, wie sich der Roboter bewegt:

1. Ändere das PWM-Tastverhältnis von 50 % auf 25 % und dann 75 % — beobachte den Geschwindigkeitsunterschied physisch. Der Roboter kriecht, dann rast er.
2. Ändere die PID-Konstanten: Verdopple Kp und beobachte Oszillation, dann erhöhe Kd, um sie zu dämpfen — die Bewegungsqualität des Roboters ändert sich sichtbar.
3. Schreibe eine Sequenz, die 2 Sekunden geradeaus fährt, 90° dreht (eine Kette vorwärts, eine rückwärts), dann wieder geradeaus fährt — der Roboter navigiert einen L-förmigen Pfad. Timing und Geschwindigkeit erzeugen Geometrie.
