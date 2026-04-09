# Stufe-1-Mission — Live-Sensor-API

## Ziel

Baue einen Python-Webserver, der die Sensordaten des Roboters als Live-JSON-API bereitstellt, abrufbar von jedem Gerät im lokalen Netzwerk.

## Anforderungen

- **Framework:** FastAPI (oder Flask)
- **Endpoints:**
  - `GET /status` → JSON mit Hostname, CPU-Temperatur, Betriebszeit
  - `GET /sensors` → JSON mit mindestens drei Sensorwerten (echt oder simuliert)
  - `WebSocket /ws/live` → sendet jede Sekunde einen Sensor-Snapshot
- **HTML-Seite** unter `GET /`, die Sensorwerte im Browser per WebSocket automatisch aktualisiert.
- Der Server bindet an `0.0.0.0`, sodass er von einem anderen Gerät im selben Netzwerk erreichbar ist.
- Code ist in dein `robot-sandbox`-Git-Repository unter `stage1-project/` committed.

## Vorgeschlagene Struktur

```
stage1-project/
├── server.py        # FastAPI-App
├── static/
│   └── index.html   # Dashboard-Seite
├── requirements.txt
└── README.md        # Wie man es startet
```

## Zusatzaufgaben

- Füge einen `POST /command`-Endpoint hinzu, der `{"cmd": "forward" | "stop" | "backward"}` akzeptiert und den Befehl mit Zeitstempel loggt.
- Zeige ein einfaches Balkendiagramm der Sensorwerte mit reinem HTML/CSS an (kein Framework nötig).
- Schreibe ein Python-Client-Skript (`client.py`), das sich mit dem WebSocket verbindet und Messwerte in eine CSV-Datei loggt.

## Reflexion

Demonstriere den laufenden Server dem Roboter und erkläre:

1. Was im Browser passiert, wenn die WebSocket-Verbindung abbricht und sich wieder verbindet.
2. Wie sich FastAPIs `async def`-Endpoint von einem regulären `def`-Endpoint unterscheidet und wann das wichtig ist.
3. Warum du diesen Server niemals ohne Authentifizierung direkt dem öffentlichen Internet aussetzen solltest.
