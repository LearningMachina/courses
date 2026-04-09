# Computer Vision

## Konzept

Eine Kamera gibt dem Roboter einen reichhaltigen, breitbandigen Blick auf die Welt. OpenCV bietet die Werkzeuge, um diese Bilder zu verarbeiten — Farben erkennen, Kanten finden, Objekte lokalisieren — und die GPU des Jetson kann die rechenintensive Arbeit über CUDA beschleunigen, was Echtzeit-Vision auch auf einem kleinen Roboter praktikabel macht.

## Themen

- Wie Kameras funktionieren: Auflösung, Bildrate, Belichtung
- OpenCV-Grundlagen: Frames aufnehmen, Farbräume, Schwellenwertbildung
- Kantenerkennung, Konturfindung
- Objekterkennung: Was YOLO tut und wie es funktioniert
- Tiefenwahrnehmung: Stereokameras und Disparitätskarten
- Vision auf dem Roboter ausführen: CUDA-Beschleunigung auf Jetson

## Code

```python
import cv2
import numpy as np

cap = cv2.VideoCapture(0)   # CSI-Kamera oder USB-Webcam

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # In HSV konvertieren und rotes Objekt per Schwellenwert erkennen
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
    lower_red = np.array([0,   120, 70])
    upper_red = np.array([10,  255, 255])
    mask = cv2.inRange(hsv, lower_red, upper_red)

    # Konturen der erkannten Region finden
    contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL,
                                   cv2.CHAIN_APPROX_SIMPLE)
    if contours:
        largest = max(contours, key=cv2.contourArea)
        x, y, w, h = cv2.boundingRect(largest)
        cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)

    # Kantenerkennung
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
# YOLO-Inferenz mit Ultralytics (GPU-beschleunigt auf Jetson)
from ultralytics import YOLO
model = YOLO("yolov8n.pt")   # Nano-Modell — schnell auf Jetson

results = model("frame.jpg", device=0)   # device=0 → CUDA-GPU
for box in results[0].boxes:
    print(box.cls, box.conf, box.xyxy)
```

## Aktion

1. Nimm Frames von der Kamera deines Roboters auf und zeige sie in einem Fenster an.
2. Erkenne per Schwellenwert ein farbiges Objekt (verwende ein Stück Klebeband oder ein Spielzeug) und zeichne einen Begrenzungsrahmen darum.
3. Gib das (x, y)-Zentrum der größten erkannten Region aus und beschreibe, ob das Objekt links, mittig oder rechts im Bild ist.
4. Führe das YOLO-Beispiel auf einem Standbild aus und gib alle erkannten Objekte mit Konfidenz > 0,5 aus.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Warum konvertieren wir für die Farberkennung nach HSV, statt BGR direkt zu verwenden?
- Was ist der Unterschied zwischen Objekterkennung und Bildklassifikation?
- Was macht die CUDA-Beschleunigung tatsächlich in der YOLO-Pipeline — welcher Schritt wird schneller?

## Erweiterung

Verändere den Vision-Code, um zu ändern, was der Roboter sieht und wie er reagiert:

1. Ändere den HSV-Farbschwellenwert, um ein blaues statt rotes Objekt zu erkennen — die Aufmerksamkeit des Roboters verschiebt sich auf verschiedene Dinge in seiner Umgebung.
2. Füge Logik hinzu, die „links", „mitte" oder „rechts" ausgibt, basierend darauf, wo das erkannte Objekt im Bild ist — der Roboter hat jetzt räumliches Bewusstsein, auf das er reagieren kann.
3. Kombiniere YOLO-Erkennung mit Motorsteuerung: Wenn eine Person erkannt wird, dreht sich der Roboter zu ihr — der Roboter verfolgt jetzt Menschen. Vision wird zu Verhalten.
