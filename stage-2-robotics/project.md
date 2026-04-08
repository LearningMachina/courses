# Stage 2 Project — Obstacle-Avoiding Robot with Camera Stream

## Goal

Build an obstacle-avoiding robot that also streams its camera feed to the browser dashboard you wrote in Stage 1.

## Requirements

**Robot behaviour**
- Drive forward continuously.
- When the front distance sensor reads < 30 cm, stop, turn, and continue.
- All drive commands flow through ROS2 topics (`/cmd_vel`).

**Camera stream**
- Capture frames from the robot's camera using OpenCV.
- Encode each frame as JPEG and push it over the WebSocket endpoint from your Stage 1 server (or add a new `/ws/camera` endpoint).
- Display the live feed in the browser dashboard.

**Integration**
- The distance reading is also pushed to the dashboard in real time.
- All code lives under `stage2-project/` in your `robot-sandbox` repo.

## Suggested Architecture

```
Jetson
├── ROS2 drive node    (obstacle avoidance logic)
├── ROS2 sensor node   (publishes ultrasonic readings)
├── ROS2 camera node   (captures frames, publishes /camera/raw)
└── FastAPI server     (bridges ROS2 → WebSocket for browser)

Browser
└── Dashboard (distance gauge + MJPEG / WebSocket frame display)
```

## Stretch Goals

- Overlay the YOLO object detection boxes on the camera stream.
- Log each obstacle event (timestamp, distance, action taken) to a CSV file.
- Add a `/control` endpoint so the user can override the robot from the browser.

## Check

Demo the fully running system to the robot and explain:

1. How the camera frames travel from the CSI sensor to the browser (every step in the pipeline).
2. What latency you observe and where the biggest delay is introduced.
3. How you would make the obstacle detection more robust (fewer false positives from sensor noise).
