# Stage 2 Mission — Build a Multiplayer Game

## Goal

Build a complete multiplayer game that combines everything from this stage: a Python 2D client, a Rust server, and 3D rendering concepts applied as visual effects.

## Requirements

- A Rust game server that accepts at least 2 players over UDP.
- A Python/Pygame client that connects to the server and renders the game state.
- Players can see each other move in real time.
- At least one game mechanic: collecting items, avoiding obstacles, or a simple combat system.
- The game has a start screen and a game-over condition.
- Frame rate is locked at 60 FPS on the client.
- The project is committed to a Git repository with a README explaining how to run it.

## Starter Template

```python
# client.py — connects to the Rust server
import pygame
import socket
import json
import sys

SERVER = ("localhost", 7878)
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.setblocking(False)

pygame.init()
screen = pygame.display.set_mode((800, 600))
clock = pygame.time.Clock()

player_name = input("Enter your name: ")
sock.sendto(json.dumps({"Join": {"name": player_name}}).encode(), SERVER)

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sock.sendto(json.dumps("Leave").encode(), SERVER)
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()
    dx = (keys[pygame.K_RIGHT] - keys[pygame.K_LEFT]) * 3.0
    dy = (keys[pygame.K_DOWN] - keys[pygame.K_UP]) * 3.0
    if dx or dy:
        sock.sendto(json.dumps({"Move": {"dx": dx, "dy": dy}}).encode(), SERVER)

    # Receive and render world state
    try:
        data, _ = sock.recvfrom(4096)
        players = json.loads(data)
        screen.fill((20, 20, 40))
        for p in players:
            color = (0, 255, 100) if p["name"] == player_name else (255, 100, 100)
            pygame.draw.circle(screen, color, (int(p["x"]) + 400, int(p["y"]) + 300), 15)
    except BlockingIOError:
        pass

    pygame.display.flip()
    clock.tick(60)
```

## Evaluation

The robot will ask you:
1. How does your server handle two players moving at the same time?
2. What happens if a player disconnects?
3. Why did you choose UDP over TCP?
4. How would you add a third dimension to this game?

## Stretch Goals

- Add a scoreboard that persists between rounds.
- Implement client-side prediction so movement feels instant.
- Add a spectator mode for additional connections.
