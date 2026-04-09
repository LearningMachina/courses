# Python-Grundlagen

## Konzept

Python ist die primäre Sprache für Robotik-Prototyping: Sie ist gut lesbar, hat umfangreiche Bibliotheken und läuft gut auf dem Jetson. Diese Grundlagen — Variablen, Kontrollfluss, Funktionen und Datenstrukturen — sind die Bausteine für jedes Programm, das du auf dem Roboter schreiben wirst.

## Themen

- Variablen, Datentypen, Operatoren
- Kontrollfluss: `if`/`else`, `for`-Schleifen, `while`-Schleifen
- Funktionen: Definition, Aufruf, Parameter, Rückgabewerte
- Listen, Dictionaries, Tupel
- Datei-I/O: Textdateien lesen und schreiben
- Module und Imports: Die Standardbibliothek nutzen
- Einführung in Klassen und Objekte
- Fehlerbehandlung: `try`/`except`, defensiven Code schreiben

## Code

```python
# Variablen und Typen
speed = 0.75          # float
name = "LearningMachina"
is_moving = False

# Kontrollfluss
if speed > 0:
    print("Roboter bewegt sich")
elif speed == 0:
    print("Roboter steht still")
else:
    print("Rückwärtsfahrt")

# Funktion
def clamp(value, low, high):
    """Wert auf den Bereich [low, high] begrenzen."""
    return max(low, min(value, high))

print(clamp(1.5, 0.0, 1.0))   # 1.0

# Liste und Dictionary
sensors = [12.3, 11.9, 13.1]
state = {"battery": 87, "temperature": 42.5}

# Datei-I/O
with open("log.txt", "w") as f:
    f.write(f"battery={state['battery']}\n")

# Klasse
class Motor:
    def __init__(self, name):
        self.name = name
        self.speed = 0.0

    def set_speed(self, speed):
        self.speed = clamp(speed, -1.0, 1.0)

left = Motor("left")
left.set_speed(0.8)
print(f"{left.name}: {left.speed}")

# Fehlerbehandlung
try:
    val = int(input("Gib eine Zahl ein: "))
except ValueError as e:
    print(f"Das war keine Zahl: {e}")
```

## Aktion

1. Schreibe eine Funktion `celsius_to_fahrenheit(c)` und teste sie mit mehreren Werten.
2. Erstelle eine `Robot`-Klasse mit `name`, `battery` (int, 0–100) und einer Methode `status()`, die eine Zusammenfassungszeile ausgibt.
3. Schreibe eine Schleife, die Zeilen aus einer Datei liest und nur Zeilen ausgibt, die mit `#` beginnen.
4. Umschließe das Dateilesen mit einem `try`/`except`, sodass eine fehlende Datei eine freundliche Fehlermeldung statt eines Tracebacks ausgibt.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist der Unterschied zwischen einer Liste und einem Dictionary? Gib jeweils ein Robotik-Beispiel.
- Warum sollte man `with open(...)` statt `f = open(...)` gefolgt von `f.close()` verwenden?
- Was passiert, wenn eine Ausnahme ausgelöst wird und kein `try`/`except` drumherum ist?

## Erweiterung

Verändere den Code, um das Verhalten des Roboters zu ändern:

1. Ändere die Variable `speed` auf einen negativen Wert — beobachte, wie sich der gemeldete Zustand des Roboters von „bewegt sich" zu „Rückwärtsfahrt" ändert.
2. Füge der `Motor`-Klasse eine Methode `battery_drain()` hinzu, die die Geschwindigkeit bei jedem Aufruf um 10 % reduziert — das Verhalten des Roboters verschlechtert sich jetzt mit der Zeit.
3. Erstelle eine Schleife, die `left.speed` schrittweise von 0 auf 1.0 erhöht — der Roboter beschleunigt. Dann kehre es um — der Roboter bremst ab. Verhalten ist jetzt eine Funktion der Zeit.
