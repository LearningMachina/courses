# Elektronik-Grundlagen

## Konzept

Bevor du irgendetwas an den Roboter anschließt, brauchst du ein mentales Modell von Elektrizität: Spannung ist der Druck, Strom ist der Fluss, und Widerstand begrenzt den Fluss. Wenn du das falsch machst, kann ein GPIO-Pin oder sogar der Jetson zerstört werden — aber wenn du es richtig machst, eröffnet sich eine Welt von Sensoren und Aktoren.

## Themen

- Spannung, Strom, Widerstand — gerade genug, um nichts kaputt zu machen
- Ein Datenblatt lesen: Was die wichtigen Zahlen wirklich bedeuten
- GPIO-Pins: Digitaler Ausgang (an/aus), digitaler Eingang, Pull-up/Pull-down
- Breadboarding: Schaltungen sicher prototypen, bevor man lötet
- Häufige Bauteile: LEDs, Taster, Widerstände, Kondensatoren
- Stromsicherheit: Warum man niemals 5 V anschließt, wo 3,3 V erwartet werden

## Code

```
Ohmsches Gesetz:  V = I × R

LED-Schaltung:
  3,3 V ──[330 Ω]──[LED]── GND

Strom = 3,3 V / 330 Ω ≈ 10 mA   ✓ sicher für GPIO
```

```python
# Einen Taster mit internem Pull-up lesen
import Jetson.GPIO as GPIO

BUTTON_PIN = 15
GPIO.setmode(GPIO.BOARD)
GPIO.setup(BUTTON_PIN, GPIO.IN, pull_up_down=GPIO.PUD_UP)

while True:
    if GPIO.input(BUTTON_PIN) == GPIO.LOW:   # gedrückt (auf GND gezogen)
        print("Taster gedrückt!")
```

```
Wichtige Datenblatt-Werte zum Prüfen:
  VCC / VDD  — Versorgungsspannung (muss zu deiner Schiene passen: 3,3 V oder 5 V)
  VIH / VIL  — Logik-HIGH/LOW-Schwellenwerte für Eingänge
  IOH / IOL  — Max. Strom, den GPIO liefern/aufnehmen kann
  Absolute Maximum Ratings — niemals überschreiten
```

## Aktion

1. Berechne den richtigen Widerstandswert für eine LED (Vf = 2,0 V) angeschlossen an 3,3 V, Zielstrom 10 mA.
2. Verkable die LED mit diesem Widerstand auf einem Breadboard und teste sie mit dem GPIO-Skript aus Stufe 0.
3. Füge einen Taster zum Breadboard hinzu und schreibe ein Skript, das die LED einschaltet, solange der Taster gehalten wird.
4. Finde das Jetson-Orin-Nano-Datenblatt und suche die maximale GPIO-Stromspezifikation.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was passiert, wenn du einen 5-V-Sensorausgang direkt an einen 3,3-V-GPIO-Eingang des Jetson anschließt?
- Was macht ein Pull-up-Widerstand, und warum wird er für einen Taster benötigt?
- Warum ist ein Kondensator nahe dem Stromeingang eines Motortreibers nützlich?

## Erweiterung

Verändere die Schaltung, um die physischen Signale des Roboters zu ändern:

1. Tausche den 330-Ω-Widerstand gegen einen 1-kΩ-Widerstand — die LED wird dunkler. Das Signal des Roboters ist jetzt leiser. Versuche 100 Ω (mit Vorsicht) — es wird lauter.
2. Füge eine zweite LED in einer anderen Farbe an einem anderen GPIO-Pin hinzu und lasse sie verschiedene Zustände signalisieren (grün = sicher, rot = Hindernis erkannt) — der Roboter kommuniziert jetzt mit Farbe.
3. Verkable einen Taster und ändere den Code, sodass die LED nur leuchtet, wenn er gedrückt wird — der Roboter reagiert jetzt auf menschliche Berührung. Das ist deine erste Eingabe-→-Ausgabe-Schleife.
