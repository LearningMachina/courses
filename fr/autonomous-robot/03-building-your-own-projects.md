# Construire vos propres projets

## Concept

Concevoir votre propre comportement de robot de zéro est le véritable test de tout ce que vous avez appris. Le processus est le même, que le projet soit simple ou complexe : définissez clairement l'objectif, décomposez-le en couches, construisez la version la plus simple qui fonctionne, puis itérez. Documenter au fur et à mesure n'est pas optionnel — le vous du futur remerciera le vous du présent.

## Thèmes

- Concevoir un comportement de robot de zéro
- Déboguer des systèmes combinant matériel et logiciel
- Documenter et partager votre projet
- Écrire des tests pour le code robotique : pourquoi le test hardware-in-the-loop est important

## Code

```
Design process for a new behaviour
─────────────────────────────────────────────
1. Define success criteria
   "The robot stops within 20 cm of any obstacle,
    with < 2 false positives per minute."

2. Identify inputs and outputs
   Inputs : ultrasonic distance, camera frame
   Outputs: motor commands, optional spoken alert

3. Sketch the control flow (state machine or behaviour tree)
   IDLE → DRIVING → OBSTACLE_DETECTED → TURNING → DRIVING

4. Build a stub: fake all sensors, test the logic first
5. Swap in real sensors one at a time
6. Run, log, fix, repeat
```

```python
# State machine skeleton
from enum import Enum, auto

class State(Enum):
    IDLE     = auto()
    DRIVING  = auto()
    AVOIDING = auto()

state = State.IDLE

def tick(distance_m: float, voice_cmd: str | None):
    global state
    if state == State.IDLE:
        if voice_cmd == "go":
            state = State.DRIVING

    elif state == State.DRIVING:
        if distance_m < 0.3:
            state = State.AVOIDING
        # drive forward

    elif state == State.AVOIDING:
        if distance_m > 0.5:
            state = State.DRIVING
        # turn

    return state
```

```python
# Unit test for the state machine (no hardware needed)
def test_obstacle_triggers_avoidance():
    import importlib, types
    # inject fake state
    assert tick(1.0, "go") == State.DRIVING
    assert tick(0.2, None) == State.AVOIDING
    assert tick(0.8, None) == State.DRIVING

test_obstacle_triggers_avoidance()
print("All tests passed")
```

## Action

1. Choisissez un comportement non couvert dans le programme et rédigez un document de conception d'une page (objectif, entrées, sorties, états).
2. Implémentez-le sous forme de machine à états avec des tests unitaires pour toutes les transitions d'états.
3. Enregistrez une vidéo de 60 secondes du robot exécutant le comportement et notez chaque défaillance.
4. Corrigez les deux défaillances principales et enregistrez à nouveau.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez à :

- Qu'est-ce que le test hardware-in-the-loop, et pourquoi ne pouvez-vous pas tester entièrement un robot physique uniquement en logiciel ?
- Comment les machines à états rendent-elles le comportement du robot plus facile à comprendre et à déboguer ?
- Que devrait contenir le README d'un projet pour que quelqu'un d'autre (ou vous, dans six mois) puisse le reproduire ?

## Extension

Modifiez la conception de votre projet pour pousser les capacités du robot plus loin :

1. Ajoutez un nouvel état à votre machine à états (par ex., SEARCHING) et implémentez les transitions — le répertoire comportemental du robot s'enrichit. Observez comment l'ajout d'états augmente la complexité.
2. Écrivez un test qui simule une défaillance de capteur (retourne `None`) et vérifiez que votre machine à états la gère correctement — le robot est maintenant robuste face aux surprises matérielles.
3. Enregistrez les transitions d'états de votre robot pendant 5 minutes et tracez-les — vous pouvez maintenant voir le « processus de pensée » du robot sous forme de chronologie. Où passe-t-il la plupart de son temps ? Pourquoi ?
