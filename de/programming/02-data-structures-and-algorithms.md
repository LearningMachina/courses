# Datenstrukturen & Algorithmen

## Konzept

Der Jetson Orin Nano ist leistungsstark, aber nicht unbegrenzt — die richtige Datenstruktur zu wählen kann den Unterschied ausmachen zwischen einem Roboter, der in Millisekunden reagiert, und einem, der hinterherhinkt. Zu verstehen, wie gängige Strukturen und Algorithmen sich verhalten, ermöglicht es dir, Code zu schreiben, der sowohl korrekt als auch schnell genug für Echtzeit ist.

## Themen

- Warum Effizienz auf eingeschränkter Hardware wichtig ist
- Listen vs Dictionaries: Wann man was nutzt
- Stacks, Queues und warum Roboter sie brauchen
- Sortieren und Suchen: Intuition hinter gängigen Algorithmen
- Big-O-Notation: Ein einfaches mentales Modell (keine Mathematik nötig)
- Rekursion: Was es ist, wann es hilft, wann es schadet

## Code

```python
from collections import deque

# Stack (LIFO) — z. B. Undo-Verlauf, Tiefensuche
stack = []
stack.append("move_forward")
stack.append("turn_left")
last = stack.pop()          # "turn_left"

# Queue (FIFO) — z. B. Sensor-Event-Puffer, Befehlswarteschlange
queue = deque()
queue.append("read_camera")
queue.append("read_lidar")
first = queue.popleft()     # "read_camera"

# Dict-Lookup ist O(1); Listensuche ist O(n)
sensor_map = {"lidar": 0, "camera": 1, "imu": 2}
idx = sensor_map["imu"]     # sofort, unabhängig von der Größe

# Binäre Suche auf sortierter Liste ist O(log n)
import bisect
distances = [1.2, 2.5, 3.8, 5.0, 7.1]
pos = bisect.bisect_left(distances, 3.5)   # Einfügeposition

# Rekursions-Beispiel: Verschachtelte Sensor-Konfiguration abflachen
def flatten(d, prefix=""):
    result = {}
    for k, v in d.items():
        key = f"{prefix}.{k}" if prefix else k
        if isinstance(v, dict):
            result.update(flatten(v, key))
        else:
            result[key] = v
    return result
```

## Aktion

1. Implementiere eine einfache Aufgabenwarteschlange für den Roboter: Befehle werden mit `enqueue(cmd)` hinzugefügt und mit `dequeue()` abgearbeitet. Verwende intern `deque`.
2. Messe bei einer Liste von 10.000 zufälligen Ganzzahlen (mit `time.perf_counter`), wie lange die Zugehörigkeitsprüfung mit einer Liste vs einem Set dauert.
3. Schreibe eine rekursive Funktion `sum_nested(lst)`, die alle Zahlen in einer beliebig verschachtelten Liste wie `[1, [2, [3, 4]], 5]` summiert.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was bedeutet O(n²) in einfachen Worten, und welcher gängige Sortieralgorithmus hat diese Komplexität im schlimmsten Fall?
- Wann würdest du eine Queue statt eines Stacks für ein Roboterverhalten wählen?
- Warum kann Rekursion auf einem Mikrocontroller oder tief eingebetteten System gefährlich sein?

## Erweiterung

Verändere den Code, um zu ändern, wie der Roboter Informationen verarbeitet:

1. Ändere die Aufgabenwarteschlange von FIFO zu LIFO (nutze sie als Stack) — beobachte, wie sich die Aufgabenausführungsreihenfolge des Roboters umkehrt. Was fühlt sich natürlicher an für interrupt-gesteuertes Verhalten?
2. Erhöhe die Testgröße von 10.000 auf 100.000 und 1.000.000 — beobachte, wie der Zeitunterschied wächst. Die Reaktionszeit des Roboters wird direkt durch deine Datenstrukturwahl beeinflusst.
3. Erweitere `sum_nested()`, sodass es auch die Verschachtelungstiefe zählt und ausgibt — der Roboter versteht jetzt die Komplexität seiner eigenen Daten.
