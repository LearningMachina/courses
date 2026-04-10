# Comportements autonomes

## Concept

Un comportement autonome est une boucle fermée qui s'exécute sans intervention humaine : le robot perçoit le monde, décide quoi faire et agit — puis recommence. Construire des comportements fiables nécessite de combiner tout ce qui a été vu dans les étapes précédentes : vision, contrôle moteur, capteurs et raisonnement par IA.

## Thèmes

- Détection de personnes et salutation
- Questions-réponses activées par la voix avec le LLM embarqué
- Navigation dans un parcours d'obstacles en utilisant la vision et le contrôle moteur
- Suivi d'une personne par flux optique
- Réponse aux commandes vocales par des actions physiques

## Code

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

## Action

1. Exécutez la boucle de détection de personnes et faites en sorte que le robot salue la première personne qu'il voit.
2. Implémentez une boucle de questions-réponses activée par la voix : écouter → transcrire (Whisper) → répondre (Ollama) → parler (Piper).
3. Construisez un parcours d'obstacles simple (chaises, boîtes) et lancez une boucle de navigation autonome à travers celui-ci.
4. Implémentez le suivi de personne par flux optique : lorsqu'une personne se déplace vers la gauche, le robot tourne à gauche pour la suivre.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez à :

- Comment gérez-vous le cas où le modèle YOLO détecte une « personne » sur une affiche ou un écran de télévision ?
- Quelle est la différence entre un comportement réactif et un comportement délibératif ?
- Comment testeriez-vous un comportement de suivi de personne en toute sécurité avant de l'exécuter dans une pièce pleine de monde ?

## Extension

Modifiez les comportements autonomes pour changer la personnalité et les capacités du robot :

1. Changez le comportement de salutation : au lieu de générer une nouvelle salutation à chaque fois, utilisez la même phrase — observez comment le robot semble moins vivant. Puis restaurez la salutation par LLM et appréciez la différence.
2. Combinez deux comportements : suivi de personne + questions-réponses vocales — le robot vous suit tout en répondant aux questions. Observez comment les comportements se composent et où les conflits apparaissent.
3. Ajoutez un mode « curieux » : lorsque le robot voit un objet inconnu (faible confiance YOLO), il s'en approche et utilise le VLM pour le décrire — le robot explore maintenant son environnement avec intention.
