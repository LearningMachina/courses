# 2D Games with Python

## Concept

Games are real-time systems — just like robots. A game loop reads input, updates state, and renders output every frame. Python with Pygame gives you the simplest path to understanding real-time programming: sprites, collision detection, physics, and animation — all visible on screen.

## Topics

- What is a game loop? Input → Update → Render
- Pygame basics: initialising a window, handling events, drawing shapes
- Sprites and images: loading, positioning, animating
- Collision detection: bounding boxes, pixel-perfect
- Simple physics: gravity, velocity, acceleration
- Sound effects and music
- Tile maps: building levels from grids
- Game state management: menus, playing, game-over

## Code

```python
import pygame
import sys

pygame.init()
screen = pygame.display.set_mode((800, 600))
clock = pygame.time.Clock()

# Player setup
player = pygame.Rect(100, 500, 40, 40)
velocity_y = 0
gravity = 0.5
on_ground = True

# Simple game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE and on_ground:
            velocity_y = -12
            on_ground = False

    # Update
    velocity_y += gravity
    player.y += velocity_y
    if player.y >= 500:
        player.y = 500
        velocity_y = 0
        on_ground = True

    # Render
    screen.fill((20, 20, 40))
    pygame.draw.rect(screen, (0, 200, 255), player)
    pygame.draw.line(screen, (100, 100, 100), (0, 540), (800, 540), 2)
    pygame.display.flip()
    clock.tick(60)
```

## Action

1. Install Pygame: `pip install pygame`.
2. Create a file `jump_game.py` with the code above.
3. Run it and press Space to make the square jump.
4. Add a second rectangle as an obstacle that moves from right to left.
5. Detect when the player collides with the obstacle and print `"Game Over!"`.

## Reflection

- What happens if you change the gravity value? The jump height?
- How is the game loop similar to a robot's sense-plan-act loop?
- Why does the game run at 60 FPS? What happens if you remove `clock.tick(60)`?

## Extension

- Add a score counter that increases every second the player survives.
- Add sprite images instead of plain rectangles.
- Create a simple particle effect when the player lands.
