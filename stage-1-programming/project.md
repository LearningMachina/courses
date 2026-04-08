# Stage 1 Project — Live Sensor API

## Goal

Build a Python web server that exposes the robot's sensor readings as a live JSON API, browsable from any device on the local network.

## Requirements

- **Framework:** FastAPI (or Flask)
- **Endpoints:**
  - `GET /status` → JSON with hostname, CPU temperature, uptime
  - `GET /sensors` → JSON with at least three sensor readings (real or simulated)
  - `WebSocket /ws/live` → pushes a sensor snapshot every second
- **HTML page** served at `GET /` that auto-updates sensor values in the browser using the WebSocket.
- The server binds to `0.0.0.0` so it is reachable from another device on the same network.
- Code is committed to your `robot-sandbox` Git repository under `stage1-project/`.

## Suggested Structure

```
stage1-project/
├── server.py        # FastAPI app
├── static/
│   └── index.html   # dashboard page
├── requirements.txt
└── README.md        # how to run it
```

## Stretch Goals

- Add a `POST /command` endpoint that accepts `{"cmd": "forward" | "stop" | "backward"}` and logs the command with a timestamp.
- Display a simple bar graph of sensor values using plain HTML/CSS (no framework needed).
- Write a Python client script (`client.py`) that connects to the WebSocket and logs readings to a CSV file.

## Check

Demo the running server to the robot and explain:

1. What happens in the browser when the WebSocket connection drops and reconnects.
2. How FastAPI's `async def` endpoint differs from a regular `def` endpoint, and when it matters.
3. Why you should never expose this server directly to the public internet without authentication.
