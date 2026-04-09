# Eigene Projekte bauen

## Konzept

Ein eigenes Roboterverhalten von Grund auf zu entwerfen, ist der wahre Test für alles, was du gelernt hast. Der Prozess ist derselbe, ob das Projekt einfach oder komplex ist: Definiere das Ziel klar, teile es in Schichten auf, baue die einfachste Version, die funktioniert, und iteriere dann. Dokumentieren während der Arbeit ist nicht optional — das zukünftige Du wird dem jetzigen Du danken.

## Themen

- Ein Roboterverhalten von Grund auf entwerfen
- Hardware- und Softwaresysteme gemeinsam debuggen
- Dein Projekt dokumentieren und teilen
- Tests für Robotercode schreiben: warum Hardware-in-the-Loop-Testen wichtig ist

## Code

```
Designprozess für ein neues Verhalten
─────────────────────────────────────────────
1. Erfolgskriterien definieren
   "Der Roboter stoppt innerhalb von 20 cm vor jedem Hindernis,
    mit < 2 Fehlalarmen pro Minute."

2. Eingaben und Ausgaben identifizieren
   Eingaben : Ultraschallentfernung, Kamerabild
   Ausgaben : Motorbefehle, optionaler gesprochener Alarm

3. Kontrollfluss skizzieren (Zustandsautomat oder Verhaltensbaum)
   IDLE → DRIVING → OBSTACLE_DETECTED → TURNING → DRIVING

4. Einen Stub bauen: alle Sensoren faken, zuerst die Logik testen
5. Echte Sensoren einzeln einsetzen
6. Laufen lassen, loggen, fixen, wiederholen
```

```python
# Zustandsautomat-Skelett
from enum import Enum, auto

class State(Enum):
    IDLE     = auto()
    DRIVING  = auto()
    AVOIDING = auto()

state = State.IDLE

def tick(distance_m: float, voice_cmd: str | None):
    global state
    if state == State.IDLE:
        if voice_cmd == "go":
            state = State.DRIVING

    elif state == State.DRIVING:
        if distance_m < 0.3:
            state = State.AVOIDING
        # vorwärts fahren

    elif state == State.AVOIDING:
        if distance_m > 0.5:
            state = State.DRIVING
        # drehen

    return state
```

```python
# Unit-Test für den Zustandsautomaten (keine Hardware nötig)
def test_obstacle_triggers_avoidance():
    import importlib, types
    # Fake-Zustand injizieren
    assert tick(1.0, "go") == State.DRIVING
    assert tick(0.2, None) == State.AVOIDING
    assert tick(0.8, None) == State.DRIVING

test_obstacle_triggers_avoidance()
print("Alle Tests bestanden")
```

## Aktion

1. Wähle ein Verhalten, das im Curriculum nicht abgedeckt ist, und schreibe ein einseitiges Designdokument (Ziel, Eingaben, Ausgaben, Zustände).
2. Implementiere es als Zustandsautomaten mit Unit-Tests für alle Zustandsübergänge.
3. Nimm ein 60-Sekunden-Video auf, wie der Roboter das Verhalten ausführt, und notiere jeden Fehler.
4. Behebe die zwei wichtigsten Fehler und nimm erneut auf.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist Hardware-in-the-Loop-Testen, und warum kann man einen physischen Roboter nicht vollständig in Software testen?
- Wie machen Zustandsautomaten Roboterverhalten einfacher zu verstehen und zu debuggen?
- Was sollte eine Projekt-README enthalten, damit jemand anders (oder du in sechs Monaten) es reproduzieren kann?

## Erweiterung

Verändere dein Projektdesign, um die Fähigkeiten des Roboters weiter zu pushen:

1. Füge einen neuen Zustand zu deinem Zustandsautomaten hinzu (z. B. SEARCHING) und implementiere die Übergänge — das Verhaltensrepertoire des Roboters wächst. Beobachte, wie das Hinzufügen von Zuständen die Komplexität erhöht.
2. Schreibe einen Test, der einen Sensorausfall simuliert (gibt `None` zurück) und verifiziere, dass dein Zustandsautomat damit elegant umgeht — der Roboter ist jetzt robust gegenüber Hardware-Überraschungen.
3. Zeichne die Zustandsübergänge deines Roboters über 5 Minuten auf und plotte sie — du kannst jetzt den „Denkprozess" des Roboters als Zeitlinie sehen. Wo verbringt er die meiste Zeit? Warum?
