# Stufe-2-Mission — Ein Multiplayer-Spiel bauen

## Ziel

Baue ein vollständiges Multiplayer-Spiel, das alles aus dieser Stufe kombiniert: einen Python-2D-Client, einen Rust-Server und 3D-Rendering-Konzepte als visuelle Effekte.

## Anforderungen

- Ein Rust-Spieleserver, der mindestens 2 Spieler über UDP akzeptiert.
- Ein Python/Pygame-Client, der sich mit dem Server verbindet und den Spielzustand rendert.
- Spieler können sich gegenseitig in Echtzeit bewegen sehen.
- Mindestens eine Spielmechanik: Gegenstände sammeln, Hindernissen ausweichen oder ein einfaches Kampfsystem.
- Das Spiel hat einen Startbildschirm und eine Game-Over-Bedingung.
- Die Bildrate ist auf dem Client auf 60 FPS begrenzt.
- Das Projekt ist in einem Git-Repository commited mit einem README, das erklärt, wie man es startet.

## Starter-Vorlage

```python
# client.py — verbindet sich mit dem Rust-Server
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

player_name = input("Gib deinen Namen ein: ")
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

## Bewertung

Der Roboter wird dich fragen:
1. Wie handhabt dein Server zwei Spieler, die sich gleichzeitig bewegen?
2. Was passiert, wenn ein Spieler die Verbindung trennt?
3. Warum hast du UDP statt TCP gewählt?
4. Wie würdest du eine dritte Dimension zu diesem Spiel hinzufügen?

## Zusatzaufgaben

- Füge eine Bestenliste hinzu, die zwischen Runden bestehen bleibt.
- Implementiere Client-seitige Vorhersage, damit sich Bewegung sofort anfühlt.
- Füge einen Zuschauer-Modus für zusätzliche Verbindungen hinzu.
