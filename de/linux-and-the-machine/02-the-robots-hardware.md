# Die Hardware des Roboters

## Konzept

Dein Roboter basiert auf dem NVIDIA Jetson Orin Nano — einem kleinen Computer mit sowohl einer CPU (für allgemeine Aufgaben) als auch einer GPU (für schnelle parallele Berechnungen, besonders KI). Die physischen Komponenten zu verstehen hilft dir, sicher zu verkabeln und zu wissen, was möglich ist, bevor du eine einzige Zeile Code schreibst.

## Themen

- Was ist ein Jetson Orin Nano? CPU vs GPU einfach erklärt
- Das LEGO-kompatible Gehäuse: Wie es funktioniert und was angebracht werden kann
- Anschlüsse und Verbindungen: USB, GPIO, UART, I2C, SPI, Kamera
- Die Motortreiberplatine: Was sie tut und wie man sie verkabelt
- Stromversorgung des Roboters: Akku, Spannungsschienen, immer ausschalten vor dem Verkabeln
- Erste Interaktion: Eine LED über die Kommandozeile einschalten

## Code

```python
# Eine LED an GPIO-Pin 18 blinken lassen mit der Jetson GPIO-Bibliothek
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
Jetson Orin Nano — wichtige Anschlüsse
──────────────────────────────────
USB-A  ×4    → Tastatur, Maus, USB-Laufwerke
USB-C  ×1    → Stromversorgung / Daten
CSI          → Flachbandkabel-Kamera (schnell, niedrige Latenz)
GPIO-Header  → 40-Pin: Strom, Masse, digitale I/O, UART, I2C, SPI
```

## Aktion

1. Bei **ausgeschaltetem** Roboter: Lokalisiere den 40-Pin-GPIO-Header und identifiziere Pin 1 (markiert mit einem Dreieck oder quadratischen Pad).
2. Finde anhand des Pinout-Diagramms einen 3,3-V-Pin, einen GND-Pin und einen GPIO-Pin.
3. Verbinde eine LED (mit einem 330-Ω-Widerstand in Reihe) zwischen einem GPIO-Pin und GND.
4. Schalte den Roboter ein und führe das obige Beispielskript aus (passe die Pin-Nummer bei Bedarf an).
5. Bestätige, dass die LED blinkt, dann `Ctrl+C` zum sauberen Beenden.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Warum muss man vor dem Verkabeln ausschalten?
- Was ist der Unterschied zwischen I2C und SPI, und wann würdest du eines dem anderen vorziehen?
- Was tut die Motortreiberplatine, das die GPIO-Pins des Jetson nicht direkt können?

## Erweiterung

Verändere den Code, um das physische Verhalten des Roboters zu ändern:

1. Ändere das Blinkintervall von 1 Sekunde auf 0,1 Sekunden — beobachte, wie sich das LED-Verhalten von einem ruhigen Puls zu einem schnellen Blitzen ändert.
2. Füge eine zweite LED an einem anderen GPIO-Pin hinzu und lasse sie abwechselnd blinken — der Roboter hat jetzt zwei sichtbare Signale.
3. Erstelle ein Muster: drei schnelle Blinker gefolgt von einer Pause. Der Roboter kommuniziert jetzt in einem einfachen Code — beobachte, wie Timing-Änderungen Bedeutung erzeugen.
