# Networking & APIs

## Concept

Robots do not live in isolation — they send sensor data to dashboards, receive commands from phones, and talk to cloud services. HTTP and WebSockets are the two most important protocols for this: HTTP for request/response (e.g., fetch a reading), WebSockets for continuous real-time streams (e.g., live camera feed metadata).

## Topics

- How computers talk: IP addresses, ports, HTTP basics
- Writing a simple HTTP server in Python (Flask / FastAPI)
- REST APIs: sending and receiving JSON
- WebSockets: real-time communication between robot and browser
- Building a minimal web dashboard for your robot's sensor data
- Node.js in one lesson: when to use JavaScript on the backend

## Code

```python
# fastapi_server.py — a minimal REST API for robot sensor data
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
# Visit http://<robot-ip>:8080/status from any device on the network
```

```python
# WebSocket example with fastapi
from fastapi import WebSocket

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    while True:
        await websocket.send_json({"cpu_temp": read_cpu_temp()})
        await asyncio.sleep(1)
```

## Action

1. Run the FastAPI example and call `/status` from your laptop's browser.
2. Add a `/sensors` endpoint that returns a JSON object with three fake sensor readings.
3. Use Python's `requests` library (or `curl`) to `POST` a JSON command `{"cmd": "stop"}` to a new `/command` endpoint and have it print the received command.
4. *(Stretch)* Open the WebSocket endpoint in a simple HTML page and display the live temperature.

## Reflection

After observing the robot's behavior, reflect on:

- What is the difference between GET and POST? When would you use each?
- Why is a WebSocket better than polling an HTTP endpoint every second for live data?
- What does `--host 0.0.0.0` do, and why is it needed to reach the robot from another device?

## Extension

Modify the API to change how the robot communicates with the world:

1. Add a `/mood` endpoint that returns a JSON mood based on sensor values (e.g., "happy" if temperature is low, "stressed" if high) — the robot now has personality visible over the network.
2. Change the WebSocket to send data every 0.5 seconds instead of 1 — observe how the dashboard feels more responsive. Then try 5 seconds — the robot feels sluggish.
3. Add a `POST /speak` endpoint that accepts a text message and makes the robot say it — any device on the network can now give the robot a voice.
