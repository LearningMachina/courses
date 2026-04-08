# Autonomous Behaviours

## Concept

An autonomous behaviour is a closed loop that runs without human input: the robot perceives the world, decides what to do, and acts — then repeats. Building reliable behaviours requires combining everything from the previous stages: vision, motor control, sensors, and AI reasoning.

## Topics

- Person detection and greeting
- Voice-activated Q&A with the onboard LLM
- Navigating an obstacle course using vision and motor control
- Following a person using optical flow
- Responding to voice commands with physical actions

## Example

```python
# Person detection + greeting behaviour (sketch)
import cv2
from ultralytics import YOLO
import ollama

model = YOLO("yolov8n.pt")
cap   = cv2.VideoCapture(0)
greeted_recently = False

while True:
    ret, frame = cap.read()
    results    = model(frame, device=0, verbose=False)

    persons = [b for b in results[0].boxes if int(b.cls) == 0 and b.conf > 0.6]
    if persons and not greeted_recently:
        reply = ollama.chat(
            model="gemma:2b",
            messages=[{"role": "user", "content": "Greet the person in one friendly sentence."}]
        )["message"]["content"]
        print(f"[ROBOT]: {reply}")
        # speak(reply)   # TTS
        greeted_recently = True
    elif not persons:
        greeted_recently = False
```

```python
# Optical flow — follow a person
import numpy as np

def optical_flow_centre(prev_gray, curr_gray):
    flow = cv2.calcOpticalFlowFarneback(
        prev_gray, curr_gray, None,
        pyr_scale=0.5, levels=3, winsize=15,
        iterations=3, poly_n=5, poly_sigma=1.2, flags=0
    )
    # Find the region with largest magnitude
    mag, ang = cv2.cartToPolar(flow[..., 0], flow[..., 1])
    _, max_loc = cv2.minMaxLoc(mag)   # (x, y) of max flow
    return max_loc
```

## Exercise

1. Run the person-detection loop and have the robot greet the first person it sees.
2. Implement a voice-activated Q&A loop: listen → transcribe (Whisper) → answer (Ollama) → speak (Piper).
3. Build a simple obstacle course (chairs, boxes) and run an autonomous navigation loop through it.
4. Implement optical flow person-following: when a person moves left, the robot turns left to track them.

## Check

Explain to the robot:

- How do you handle the case where the YOLO model detects a "person" in a poster or a TV screen?
- What is the difference between reactive behaviour and deliberative behaviour?
- How would you test a person-following behaviour safely before running it in a room full of people?
