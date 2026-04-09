# Stufe-2-Mission — Hindernisausweichender Roboter mit Kamerastream

## Ziel

Baue einen hindernisausweichenden Roboter, der gleichzeitig seinen Kamerafeed an das Browser-Dashboard streamt, das du in Stufe 1 gebaut hast.

## Anforderungen

**Roboterverhalten**
- Kontinuierlich vorwärts fahren.
- Wenn der vordere Entfernungssensor < 30 cm misst, stoppen, drehen und weiterfahren.
- Alle Fahrbefehle fließen über ROS2-Topics (`/cmd_vel`).

**Kamerastream**
- Frames von der Kamera des Roboters mit OpenCV aufnehmen.
- Jeden Frame als JPEG codieren und über den WebSocket-Endpoint deines Stufe-1-Servers senden (oder einen neuen `/ws/camera`-Endpoint hinzufügen).
- Den Live-Feed im Browser-Dashboard anzeigen.

**Integration**
- Der Entfernungswert wird ebenfalls in Echtzeit an das Dashboard gesendet.
- Der gesamte Code liegt unter `stage2-project/` in deinem `robot-sandbox`-Repo.

## Vorgeschlagene Architektur

```
Jetson
├── ROS2 Drive-Node    (Hindernisvermeidungslogik)
├── ROS2 Sensor-Node   (veröffentlicht Ultraschallwerte)
├── ROS2 Kamera-Node   (nimmt Frames auf, veröffentlicht /camera/raw)
└── FastAPI-Server     (Brücke ROS2 → WebSocket für Browser)

Browser
└── Dashboard (Entfernungsanzeige + MJPEG / WebSocket-Frame-Anzeige)
```

## Zusatzaufgaben

- YOLO-Objekterkennungsrahmen auf den Kamerastream überlagern.
- Jedes Hindernis-Ereignis (Zeitstempel, Entfernung, durchgeführte Aktion) in eine CSV-Datei loggen.
- Einen `/control`-Endpoint hinzufügen, damit der Benutzer den Roboter vom Browser aus steuern kann.

## Reflexion

Demonstriere das vollständig laufende System dem Roboter und erkläre:

1. Wie die Kameraframes vom CSI-Sensor zum Browser gelangen (jeden Schritt in der Pipeline).
2. Welche Latenz du beobachtest und wo die größte Verzögerung eingeführt wird.
3. Wie du die Hinderniserkennung robuster machen würdest (weniger Fehlalarme durch Sensorrauschen).
