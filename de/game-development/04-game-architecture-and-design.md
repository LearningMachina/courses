# Spielearchitektur & Design

## Konzept

Ein echtes Spiel ist mehr als Rendering und Netzwerk — es braucht Struktur. Spielearchitektur definiert, wie Systeme kommunizieren: das Entity-Component-System (ECS) Muster, Event-Busse, Asset-Pipelines und Zustandsmaschinen. Gute Architektur bedeutet, dass dein Spiel handhabbar bleibt, wenn es vom Prototyp zu einem vollständigen Erlebnis wächst.

## Themen

- Entity-Component-System (ECS): Daten von Verhalten trennen
- Spielzustandsmaschinen: Titelbildschirm → Gameplay → Pause → Game Over
- Event-Systeme: Eingabe von Spiellogik entkoppeln
- Asset-Verwaltung: Texturen, Sounds und Modelle effizient laden
- Szenengraphen: Objekte in einer Hierarchie organisieren
- Fester vs variabler Zeitschritt: Physik bildratenunabhängig machen
- Debug-Werkzeuge: In-Game-Konsolen, Frame-Profiler, Debug-Overlays
- Sprachübergreifende Integration: Rust von Python aufrufen, C++ von Python

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

## Aktion

1. Erstelle eine Datei `ecs_demo.py` mit dem obigen Code und führe sie aus.
2. Füge ein `collision_system` hinzu, das prüft, ob sich zwei Entities mit `position` und `sprite` überlappen.
3. Implementiere eine einfache Zustandsmaschine: ein `GameState`-Enum mit `MENU`, `PLAYING`, `GAME_OVER`.
4. Verbinde die Zustandsmaschine so, dass Enter im `MENU` zu `PLAYING` wechselt.

## Reflexion

- Warum Daten (Components) von Verhalten (Systeme) trennen?
- Wie ähnelt ECS der Art, wie ROS2 Nodes und Topics trennt?
- Welche Probleme würdest du ohne Zustandsmaschine erleben?

## Erweiterung

- Implementiere das ECS-Muster in Rust mit Structs und Vektoren.
- Füge einen Event-Bus hinzu: Systeme senden Events (`SpielerGetroffen`, `GegnerZerstört`) und andere Systeme reagieren.
- Baue ein Debug-Overlay, das Entity-Anzahl und FPS auf dem Bildschirm anzeigt.
