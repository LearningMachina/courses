# Game Architecture & Design

## Concept

A real game is more than rendering and networking — it needs structure. Game architecture defines how systems communicate: the Entity-Component-System (ECS) pattern, event buses, asset pipelines, and state machines. Good architecture means your game stays manageable as it grows from a prototype to a complete experience.

## Topics

- Entity-Component-System (ECS): separating data from behaviour
- Game state machines: title screen → gameplay → pause → game over
- Event systems: decoupling input from game logic
- Asset management: loading textures, sounds, and models efficiently
- Scene graphs: organising objects in a hierarchy
- Fixed vs variable timestep: making physics frame-rate independent
- Debugging tools: in-game consoles, frame profilers, debug overlays
- Cross-language integration: calling Rust from Python, C++ from Python

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

1. Create a file `ecs_demo.py` with the code above and run it.
2. Add a `collision_system` that checks if any two entities with `position` and `sprite` overlap.
3. Implement a simple state machine: a `GameState` enum with `MENU`, `PLAYING`, `GAME_OVER`.
4. Wire the state machine so pressing Enter in `MENU` transitions to `PLAYING`.

## Reflection

- Why separate data (components) from behaviour (systems)?
- How is ECS similar to how ROS2 separates nodes and topics?
- What problems would you encounter without a state machine?

## Extension

- Implement the ECS pattern in Rust using structs and vectors.
- Add an event bus: systems emit events (`PlayerHit`, `EnemyDestroyed`) and other systems react.
- Build a debug overlay that prints entity counts and FPS to the screen.
