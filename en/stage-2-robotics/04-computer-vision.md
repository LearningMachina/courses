# Computer Vision

## Concept

A camera gives the robot a rich, high-bandwidth view of the world. OpenCV provides the tools to process those images — detect colours, find edges, locate objects — and the Jetson's GPU can accelerate the heavy work via CUDA, making real-time vision practical even on a small robot.

## Topics

- How cameras work: resolution, frame rate, exposure
- OpenCV basics: capturing frames, colour spaces, thresholding
- Edge detection, contour finding
- Object detection: what YOLO does and how it works
- Depth perception: stereo cameras and disparity maps
- Running vision on the robot: CUDA acceleration on Jetson

## Code

```python
import cv2
import numpy as np

cap = cv2.VideoCapture(0)   # CSI camera or USB webcam

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # Convert to HSV and threshold for a red object
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
    lower_red = np.array([0,   120, 70])
    upper_red = np.array([10,  255, 255])
    mask = cv2.inRange(hsv, lower_red, upper_red)

    # Find contours of the detected region
    contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL,
                                   cv2.CHAIN_APPROX_SIMPLE)
    if contours:
        largest = max(contours, key=cv2.contourArea)
        x, y, w, h = cv2.boundingRect(largest)
        cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)

    # Edge detection
    gray  = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    edges = cv2.Canny(gray, threshold1=50, threshold2=150)

    cv2.imshow("frame", frame)
    cv2.imshow("edges", edges)
    if cv2.waitKey(1) & 0xFF == ord("q"):
        break

cap.release()
cv2.destroyAllWindows()
```

```python
# YOLO inference with Ultralytics (GPU-accelerated on Jetson)
from ultralytics import YOLO
model = YOLO("yolov8n.pt")   # nano model — fast on Jetson

results = model("frame.jpg", device=0)   # device=0 → CUDA GPU
for box in results[0].boxes:
    print(box.cls, box.conf, box.xyxy)
```

## Action

1. Capture frames from your robot's camera and display them in a window.
2. Threshold for a brightly coloured object (use a piece of tape or a toy) and draw a bounding box around it.
3. Print the (x, y) centre of the largest detected region and describe whether the object is left, centre, or right of the frame.
4. Run the YOLO example on a still image and print all detected objects with confidence > 0.5.

## Reflection

After observing the robot's behavior, reflect on:

- Why do we convert to HSV for colour detection instead of using BGR directly?
- What is the difference between object detection and image classification?
- What does CUDA acceleration actually do in the YOLO pipeline — which step gets faster?

## Extension

Modify the vision code to change what the robot sees and how it reacts:

1. Change the HSV colour threshold to detect a blue object instead of red — the robot's attention shifts to different things in its environment.
2. Add logic that prints "left", "center", or "right" based on where the detected object is in the frame — the robot now has spatial awareness it can act on.
3. Combine YOLO detection with motor control: when a person is detected, the robot turns toward them — the robot now tracks humans. Vision becomes behavior.
