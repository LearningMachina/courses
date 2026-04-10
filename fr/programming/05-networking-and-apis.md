# Réseaux et API

## Concept

Les robots ne vivent pas en isolation — ils envoient des données de capteurs à des tableaux de bord, reçoivent des commandes depuis des téléphones et communiquent avec des services cloud. HTTP et WebSocket sont les deux protocoles les plus importants pour cela : HTTP pour les requêtes/réponses (par exemple, récupérer une mesure), WebSocket pour les flux continus en temps réel (par exemple, les métadonnées d'un flux caméra en direct).

## Thèmes

- Comment les ordinateurs communiquent : adresses IP, ports, bases de HTTP
- Écriture d'un serveur HTTP simple en Python (Flask / FastAPI)
- REST API : envoi et réception de JSON
- WebSocket : communication en temps réel entre le robot et le navigateur
- Construction d'un tableau de bord web minimal pour les données des capteurs de votre robot
- Node.js en une leçon : quand utiliser JavaScript côté serveur

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

1. Exécutez l'exemple FastAPI et appelez `/status` depuis le navigateur de votre ordinateur portable.
2. Ajoutez un endpoint `/sensors` qui retourne un objet JSON avec trois lectures de capteurs fictives.
3. Utilisez la bibliothèque Python `requests` (ou `curl`) pour envoyer une commande JSON `{"cmd": "stop"}` en `POST` vers un nouvel endpoint `/command` et faites-lui afficher la commande reçue.
4. *(Défi supplémentaire)* Ouvrez l'endpoint WebSocket dans une page HTML simple et affichez la température en direct.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Quelle est la différence entre GET et POST ? Quand utiliseriez-vous chacun ?
- Pourquoi un WebSocket est-il préférable à l'interrogation répétée d'un endpoint HTTP chaque seconde pour des données en temps réel ?
- Que fait `--host 0.0.0.0`, et pourquoi est-ce nécessaire pour accéder au robot depuis un autre appareil ?

## Extension

Modifiez l'API pour changer la façon dont le robot communique avec le monde :

1. Ajoutez un endpoint `/mood` qui retourne une humeur en JSON basée sur les valeurs des capteurs (par exemple, « joyeux » si la température est basse, « stressé » si elle est élevée) — le robot a désormais une personnalité visible sur le réseau.
2. Changez le WebSocket pour envoyer des données toutes les 0,5 secondes au lieu de 1 — observez comment le tableau de bord semble plus réactif. Puis essayez 5 secondes — le robot semble lent.
3. Ajoutez un endpoint `POST /speak` qui accepte un message texte et fait parler le robot — n'importe quel appareil sur le réseau peut désormais donner une voix au robot.
