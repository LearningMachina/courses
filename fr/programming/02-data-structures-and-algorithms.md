# Structures de données et algorithmes

## Concept

Le Jetson Orin Nano est puissant mais pas infini — choisir la bonne structure de données peut faire la différence entre un robot qui répond en millisecondes et un robot qui rame. Comprendre le comportement des structures et algorithmes courants vous permet d'écrire du code à la fois correct et suffisamment rapide pour fonctionner en temps réel.

## Thèmes

- Pourquoi l'efficacité est importante sur du matériel limité
- Listes vs dictionnaires : quand utiliser lequel
- Piles, files d'attente, et pourquoi les robots les utilisent
- Tri et recherche : intuition derrière les algorithmes courants
- Notation Big-O : un modèle mental simple (aucune mathématique requise)
- Récursivité : ce que c'est, quand ça aide, quand ça nuit

## Code

```python
from collections import deque

# Stack (LIFO) — e.g. undo history, depth-first search
stack = []
stack.append("move_forward")
stack.append("turn_left")
last = stack.pop()          # "turn_left"

# Queue (FIFO) — e.g. sensor event buffer, command queue
queue = deque()
queue.append("read_camera")
queue.append("read_lidar")
first = queue.popleft()     # "read_camera"

# Dict lookup is O(1); list search is O(n)
sensor_map = {"lidar": 0, "camera": 1, "imu": 2}
idx = sensor_map["imu"]     # instant, regardless of size

# Binary search on a sorted list is O(log n)
import bisect
distances = [1.2, 2.5, 3.8, 5.0, 7.1]
pos = bisect.bisect_left(distances, 3.5)   # insert position

# Recursion example: flatten nested sensor config
def flatten(d, prefix=""):
    result = {}
    for k, v in d.items():
        key = f"{prefix}.{k}" if prefix else k
        if isinstance(v, dict):
            result.update(flatten(v, key))
        else:
            result[key] = v
    return result
```

## Action

1. Implémentez une file de tâches simple pour le robot : les commandes sont ajoutées avec `enqueue(cmd)` et consommées avec `dequeue()`. Utilisez `deque` en interne.
2. À partir d'une liste de 10 000 entiers aléatoires, mesurez (avec `time.perf_counter`) le temps nécessaire pour vérifier l'appartenance avec une liste par rapport à un ensemble (set).
3. Écrivez une fonction récursive `sum_nested(lst)` qui additionne tous les nombres dans une liste imbriquée arbitrairement, comme `[1, [2, [3, 4]], 5]`.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Que signifie O(n²) en langage courant, et quel algorithme de tri courant a cette complexité dans le pire des cas ?
- Quand choisiriez-vous une file d'attente plutôt qu'une pile pour un comportement robotique ?
- Pourquoi la récursivité peut-elle être dangereuse sur un microcontrôleur ou un système profondément embarqué ?

## Extension

Modifiez le code pour changer la façon dont le robot traite l'information :

1. Changez la file de tâches de FIFO à LIFO (utilisez-la comme une pile) — observez comment l'ordre d'exécution des tâches du robot s'inverse. Lequel semble plus naturel pour un comportement piloté par interruptions ?
2. Augmentez la taille du test de 10 000 à 100 000 puis 1 000 000 — observez la différence de temps croître. Le temps de réponse du robot est directement affecté par votre choix de structure de données.
3. Modifiez `sum_nested()` pour compter également la profondeur d'imbrication et l'afficher — le robot comprend maintenant la complexité de ses propres données.
