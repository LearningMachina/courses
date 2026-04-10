# Architecture et conception de jeux

## Concept

Un vrai jeu, c'est bien plus que du rendu et du réseau — il a besoin de structure. L'architecture de jeu définit comment les systèmes communiquent : le patron Entity-Component-System (ECS), les bus d'événements, les pipelines de ressources et les machines à états. Une bonne architecture signifie que votre jeu reste gérable à mesure qu'il passe d'un prototype à une expérience complète.

## Thèmes

- Entity-Component-System (ECS) : séparer les données du comportement
- Machines à états du jeu : écran titre → gameplay → pause → fin de partie
- Systèmes d'événements : découpler les entrées de la logique du jeu
- Gestion des ressources : chargement efficace des textures, sons et modèles
- Graphes de scène : organiser les objets dans une hiérarchie
- Pas de temps fixe vs variable : rendre la physique indépendante du taux d'images
- Outils de débogage : consoles en jeu, profileurs d'images, superpositions de débogage
- Intégration multi-langage : appeler Rust depuis Python, C++ depuis Python

## Code

```python
# Simple ECS pattern in Python
from dataclasses import dataclass, field
from typing import Dict, List, Any

@dataclass
class Entity:
    id: int
    components: Dict[str, Any] = field(default_factory=dict)

class World:
    def __init__(self):
        self.entities: List[Entity] = []
        self.next_id = 0

    def spawn(self, **components) -> Entity:
        entity = Entity(id=self.next_id, components=components)
        self.next_id += 1
        self.entities.append(entity)
        return entity

    def query(self, *component_names):
        for entity in self.entities:
            if all(c in entity.components for c in component_names):
                yield entity

# Create a game world
world = World()
player = world.spawn(position=(100, 200), velocity=(0, 0), sprite="hero.png")
enemy = world.spawn(position=(400, 200), velocity=(-1, 0), sprite="enemy.png", health=3)

# Movement system
def movement_system(world):
    for entity in world.query("position", "velocity"):
        pos = entity.components["position"]
        vel = entity.components["velocity"]
        entity.components["position"] = (pos[0] + vel[0], pos[1] + vel[1])

# Run one tick
movement_system(world)
print(f"Player: {player.components['position']}")
print(f"Enemy: {enemy.components['position']}")
```

## Action

1. Créez un fichier `ecs_demo.py` avec le code ci-dessus et exécutez-le.
2. Ajoutez un `collision_system` qui vérifie si deux entités ayant `position` et `sprite` se chevauchent.
3. Implémentez une machine à états simple : une énumération `GameState` avec `MENU`, `PLAYING`, `GAME_OVER`.
4. Connectez la machine à états de sorte qu'appuyer sur Entrée dans `MENU` déclenche la transition vers `PLAYING`.

## Réflexion

- Pourquoi séparer les données (composants) du comportement (systèmes) ?
- En quoi ECS est-il similaire à la façon dont ROS2 sépare les nœuds et les topics ?
- Quels problèmes rencontreriez-vous sans machine à états ?

## Extension

- Implémentez le patron ECS en Rust en utilisant des structs et des vecteurs.
- Ajoutez un bus d'événements : les systèmes émettent des événements (`PlayerHit`, `EnemyDestroyed`) et d'autres systèmes réagissent.
- Construisez une superposition de débogage qui affiche le nombre d'entités et les FPS à l'écran.
