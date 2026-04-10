# Mission de l'étape 2 — Robot éviteur d'obstacles avec flux caméra

## Objectif

Construire un robot éviteur d'obstacles qui diffuse également le flux de sa caméra vers le tableau de bord web que vous avez écrit à l'étape 1.

## Exigences

**Comportement du robot**
- Avancer en continu.
- Lorsque le capteur de distance frontal indique < 30 cm, s'arrêter, tourner et continuer.
- Toutes les commandes de déplacement passent par les topics ROS2 (`/cmd_vel`).

**Flux caméra**
- Capturer les images depuis la caméra du robot en utilisant OpenCV.
- Encoder chaque image en JPEG et l'envoyer via le point de terminaison WebSocket de votre serveur de l'étape 1 (ou ajouter un nouveau point de terminaison `/ws/camera`).
- Afficher le flux en direct dans le tableau de bord web.

**Intégration**
- La mesure de distance est également transmise au tableau de bord en temps réel.
- Tout le code se trouve dans `stage2-project/` de votre dépôt `robot-sandbox`.

## Architecture suggérée

```
Jetson
├── ROS2 drive node    (obstacle avoidance logic)
├── ROS2 sensor node   (publishes ultrasonic readings)
├── ROS2 camera node   (captures frames, publishes /camera/raw)
└── FastAPI server     (bridges ROS2 → WebSocket for browser)

Browser
└── Dashboard (distance gauge + MJPEG / WebSocket frame display)
```

## Objectifs supplémentaires

- Superposer les rectangles de détection YOLO sur le flux caméra.
- Enregistrer chaque événement d'obstacle (horodatage, distance, action effectuée) dans un fichier CSV.
- Ajouter un point de terminaison `/control` pour que l'utilisateur puisse contrôler le robot depuis le navigateur.

## Réflexion

Faites une démonstration du système complet en fonctionnement et expliquez :

1. Comment les images de la caméra voyagent du capteur CSI jusqu'au navigateur (chaque étape du pipeline).
2. Quelle latence vous observez et où le plus grand délai est introduit.
3. Comment vous rendriez la détection d'obstacles plus robuste (moins de faux positifs liés au bruit des capteurs).
