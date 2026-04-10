# Architecture complète du système

## Concept

Un robot autonome est un système en couches : les données brutes des capteurs remontent à travers les couches de perception et de raisonnement, et les commandes redescendent vers les actionneurs. Chaque couche introduit de la latence, il est donc essentiel de comprendre où le temps est dépensé — et quelles sont les contraintes temps réel — avant d'essayer d'optimiser.

## Thèmes

- Comment toutes les couches se connectent : capteurs → perception → raisonnement → action
- Latence et contraintes temps réel
- Tout exécuter hors ligne : pas de cloud, pas d'abonnement
- Profilage et optimisation : trouver les goulots d'étranglement sur le matériel Jetson

## Code

```
Full stack on one Jetson Orin Nano
────────────────────────────────────────────────────────
Layer           Component               Typical latency
────────────────────────────────────────────────────────
Sensors         Ultrasonic, IMU         1–10 ms
Perception      OpenCV, YOLO            30–80 ms (GPU)
Language model  Ollama / llama.cpp      200–800 ms
Motor control   ROS2 node → PWM         1–5 ms
TTS / STT       Piper / Whisper-tiny    100–400 ms
────────────────────────────────────────────────────────
Total round-trip (voice command → action): ~500 ms–1.5 s
```

```python
# Profile a pipeline stage with Python's cProfile
import cProfile, pstats, io

def perception_pipeline(frame):
    # ... your OpenCV / YOLO code ...
    pass

pr = cProfile.Profile()
pr.enable()
perception_pipeline(frame)
pr.disable()

s = io.StringIO()
pstats.Stats(pr, stream=s).sort_stats("cumulative").print_stats(10)
print(s.getvalue())
```

```bash
# Jetson system monitoring
tegrastats                      # GPU / CPU / memory / power live
jtop                            # rich TUI (pip install jetson-stats)
sudo nvpmodel -m 0              # max performance mode
sudo jetson_clocks              # lock clocks to maximum
```

## Action

1. Dessinez un diagramme en blocs de l'ensemble de la pile de votre robot, en identifiant chaque composant et les données circulant entre eux.
2. Utilisez `tegrastats` pendant l'exécution de YOLO et relevez l'utilisation du GPU, l'utilisation de la mémoire et la consommation électrique.
3. Profilez le pipeline de perception avec `cProfile` et identifiez les trois appels de fonction les plus coûteux.
4. Alternez entre les modes d'alimentation du Jetson (`nvpmodel`) et mesurez le changement du temps d'inférence de YOLO.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez à :

- Que signifie « temps réel » dans un contexte robotique, et quelles couches de votre pile doivent respecter des contraintes temps réel strictes ?
- Pourquoi est-il important de fonctionner hors ligne sans dépendance au cloud ? Citez deux scénarios où la dépendance au cloud échouerait.
- Quelle est la différence entre la latence et le débit, et lequel est le plus important pour l'évitement d'obstacles par rapport à l'interaction vocale ?

## Extension

Modifiez l'architecture du système pour changer les caractéristiques de performance du robot :

1. Changez le mode d'alimentation du Jetson avec `nvpmodel` et relancez votre benchmark YOLO — observez comment le robot échange la consommation d'énergie contre la vitesse. En mode basse consommation, il voit le monde au ralenti.
2. Ajoutez un horodatage à chaque couche (lecture capteur, perception, raisonnement, commande moteur) et calculez la latence de bout en bout — le robot sait maintenant à quelle vitesse il pense. Identifiez le goulot d'étranglement et essayez de le réduire.
3. Désactivez une couche (par ex., ignorez le raisonnement LLM) et observez comment le comportement du robot se dégrade vers un contrôle purement réactif — comprendre ce que chaque couche apporte rend l'ensemble du système plus clair.
