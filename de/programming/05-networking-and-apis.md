# Netzwerk & APIs

## Konzept

Roboter leben nicht isoliert — sie senden Sensordaten an Dashboards, empfangen Befehle von Handys und kommunizieren mit Cloud-Diensten. HTTP und WebSockets sind die zwei wichtigsten Protokolle dafür: HTTP für Anfrage/Antwort (z. B. einen Messwert abrufen), WebSockets für kontinuierliche Echtzeit-Streams (z. B. Live-Kamerafeed-Metadaten).

## Themen

- Wie Computer kommunizieren: IP-Adressen, Ports, HTTP-Grundlagen
- Einen einfachen HTTP-Server in Python schreiben (Flask / FastAPI)
- REST-APIs: JSON senden und empfangen
- WebSockets: Echtzeit-Kommunikation zwischen Roboter und Browser
- Ein minimales Web-Dashboard für die Sensordaten deines Roboters bauen
- Node.js in einer Lektion: Wann man JavaScript im Backend nutzt

## Code

```python
# fastapi_server.py — eine minimale REST-API für Roboter-Sensordaten
from fastapi import FastAPI
from fastapi.responses import HTMLResponse
import random, time

app = FastAPI()

def read_cpu_temp():
    try:
        raw = open("/sys/class/thermal/thermal_zone0/temp").read()
        return round(int(raw) / 1000, 1)
    except Exception:
        return round(random.uniform(40.0, 60.0), 1)

@app.get("/status")
def get_status():
    return {
        "hostname": "learningmachina",
        "cpu_temp": read_cpu_temp(),
        "timestamp": time.time(),
    }
```

```bash
pip install fastapi uvicorn
uvicorn fastapi_server:app --host 0.0.0.0 --port 8080 --reload
# Besuche http://<roboter-ip>:8080/status von jedem Gerät im Netzwerk
```

```python
# WebSocket-Beispiel mit FastAPI
from fastapi import WebSocket

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    while True:
        await websocket.send_json({"cpu_temp": read_cpu_temp()})
        await asyncio.sleep(1)
```

## Aktion

1. Starte das FastAPI-Beispiel und rufe `/status` vom Browser deines Laptops auf.
2. Füge einen `/sensors`-Endpoint hinzu, der ein JSON-Objekt mit drei simulierten Sensorwerten zurückgibt.
3. Nutze Pythons `requests`-Bibliothek (oder `curl`), um einen JSON-Befehl `{"cmd": "stop"}` per `POST` an einen neuen `/command`-Endpoint zu senden, und lasse ihn den empfangenen Befehl ausgeben.
4. *(Zusatz)* Öffne den WebSocket-Endpoint in einer einfachen HTML-Seite und zeige die Live-Temperatur an.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist der Unterschied zwischen GET und POST? Wann würdest du jeweils welches verwenden?
- Warum ist ein WebSocket besser als das Polling eines HTTP-Endpoints jede Sekunde für Live-Daten?
- Was bewirkt `--host 0.0.0.0`, und warum ist es nötig, um den Roboter von einem anderen Gerät zu erreichen?

## Erweiterung

Verändere die API, um zu ändern, wie der Roboter mit der Welt kommuniziert:

1. Füge einen `/mood`-Endpoint hinzu, der eine JSON-Stimmung basierend auf Sensorwerten zurückgibt (z. B. „happy" bei niedriger Temperatur, „stressed" bei hoher) — der Roboter hat jetzt Persönlichkeit, sichtbar über das Netzwerk.
2. Ändere den WebSocket, sodass er alle 0,5 Sekunden statt jede Sekunde Daten sendet — beobachte, wie sich das Dashboard reaktionsschneller anfühlt. Versuche dann 5 Sekunden — der Roboter fühlt sich träge an.
3. Füge einen `POST /speak`-Endpoint hinzu, der eine Textnachricht akzeptiert und den Roboter sprechen lässt — jedes Gerät im Netzwerk kann dem Roboter jetzt eine Stimme geben.
