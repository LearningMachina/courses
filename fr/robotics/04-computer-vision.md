# Vision par ordinateur

## Concept

Une caméra offre au robot une vue riche et à large bande passante du monde. OpenCV fournit les outils pour traiter ces images — détecter des couleurs, trouver des contours, localiser des objets — et le GPU du Jetson peut accélérer les traitements lourds via CUDA, rendant la vision en temps réel possible même sur un petit robot.

## Thèmes

- Fonctionnement des caméras : résolution, fréquence d'images, exposition
- Les bases d'OpenCV : capture d'images, espaces colorimétriques, seuillage
- Détection de contours, recherche de formes
- Détection d'objets : ce que fait YOLO et comment il fonctionne
- Perception de la profondeur : caméras stéréo et cartes de disparité
- Exécuter la vision sur le robot : accélération CUDA sur Jetson

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

1. Capturez des images depuis la caméra de votre robot et affichez-les dans une fenêtre.
2. Appliquez un seuillage pour détecter un objet de couleur vive (utilisez un morceau de ruban adhésif ou un jouet) et dessinez un rectangle englobant autour de celui-ci.
3. Affichez le centre (x, y) de la plus grande région détectée et indiquez si l'objet se trouve à gauche, au centre ou à droite de l'image.
4. Exécutez l'exemple YOLO sur une image fixe et affichez tous les objets détectés avec une confiance > 0,5.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Pourquoi convertit-on en HSV pour la détection de couleurs au lieu d'utiliser directement le BGR ?
- Quelle est la différence entre la détection d'objets et la classification d'images ?
- Que fait concrètement l'accélération CUDA dans le pipeline YOLO — quelle étape devient plus rapide ?

## Extension

Modifiez le code de vision pour changer ce que le robot voit et comment il réagit :

1. Changez le seuil de couleur HSV pour détecter un objet bleu au lieu de rouge — l'attention du robot se porte sur des éléments différents de son environnement.
2. Ajoutez une logique qui affiche « gauche », « centre » ou « droite » selon l'emplacement de l'objet détecté dans l'image — le robot a maintenant une conscience spatiale sur laquelle il peut agir.
3. Combinez la détection YOLO avec le contrôle des moteurs : lorsqu'une personne est détectée, le robot se tourne vers elle — le robot suit désormais les humains. La vision devient un comportement.
