# Autonome Verhaltensweisen

## Konzept

Ein autonomes Verhalten ist eine geschlossene Schleife, die ohne menschlichen Eingriff läuft: Der Roboter nimmt die Welt wahr, entscheidet, was zu tun ist, und handelt — dann wiederholt er. Zuverlässige Verhaltensweisen zu bauen erfordert, alles aus den vorherigen Stufen zu kombinieren: Vision, Motorsteuerung, Sensoren und KI-Denken.

## Themen

- Personenerkennung und Begrüßung
- Sprachaktivierte Frage-Antwort mit dem Onboard-LLM
- Einen Hindernisparcours mit Vision und Motorsteuerung navigieren
- Einer Person mit optischem Fluss folgen
- Auf Sprachbefehle mit physischen Aktionen reagieren

## Code

```python
# Personenerkennung + Begrüßungsverhalten (Skizze)
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
            messages=[{"role": "user", "content": "Begrüße die Person in einem freundlichen Satz."}]
        )["message"]["content"]
        print(f"[ROBOTER]: {reply}")
        # speak(reply)   # TTS
        greeted_recently = True
    elif not persons:
        greeted_recently = False
```

```python
# Optischer Fluss — einer Person folgen
import numpy as np

def optical_flow_centre(prev_gray, curr_gray):
    flow = cv2.calcOpticalFlowFarneback(
        prev_gray, curr_gray, None,
        pyr_scale=0.5, levels=3, winsize=15,
        iterations=3, poly_n=5, poly_sigma=1.2, flags=0
    )
    # Region mit der größten Magnitude finden
    mag, ang = cv2.cartToPolar(flow[..., 0], flow[..., 1])
    _, max_loc = cv2.minMaxLoc(mag)   # (x, y) des maximalen Flusses
    return max_loc
```

## Aktion

1. Starte die Personenerkennungsschleife und lass den Roboter die erste Person begrüßen, die er sieht.
2. Implementiere eine sprachaktivierte Frage-Antwort-Schleife: Zuhören → Transkribieren (Whisper) → Antworten (Ollama) → Sprechen (Piper).
3. Baue einen einfachen Hindernisparcours (Stühle, Kisten) und lass den Roboter autonom hindurch navigieren.
4. Implementiere optische-Fluss-Personenverfolgung: Wenn eine Person sich nach links bewegt, dreht der Roboter nach links, um sie zu verfolgen.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Wie gehst du mit dem Fall um, dass das YOLO-Modell eine „Person" auf einem Poster oder einem Fernsehbildschirm erkennt?
- Was ist der Unterschied zwischen reaktivem Verhalten und deliberativem Verhalten?
- Wie würdest du ein Personenverfolgungsverhalten sicher testen, bevor du es in einem Raum voller Menschen laufen lässt?

## Erweiterung

Verändere die autonomen Verhaltensweisen, um Persönlichkeit und Fähigkeiten des Roboters zu ändern:

1. Ändere das Begrüßungsverhalten: Statt jedes Mal eine neue Begrüßung zu generieren, verwende denselben Satz — beobachte, wie der Roboter weniger lebendig wirkt. Stelle dann die LLM-Begrüßung wieder her und schätze den Unterschied.
2. Kombiniere zwei Verhaltensweisen: Personenverfolgung + Sprach-Q&A — der Roboter folgt dir, während er Fragen beantwortet. Beobachte, wie Verhaltensweisen sich zusammensetzen und wo Konflikte entstehen.
3. Füge einen „neugierigen" Modus hinzu: Wenn der Roboter ein unbekanntes Objekt sieht (niedrige YOLO-Konfidenz), fährt er näher heran und nutzt das VLM, um es zu beschreiben — der Roboter erkundet jetzt seine Umgebung mit Absicht.
