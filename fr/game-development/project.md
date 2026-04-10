# Mission de l'étape 2 — Construire un jeu multijoueur

## Objectif

Construisez un jeu multijoueur complet qui combine tout ce qui a été vu dans cette étape : un client 2D en Python, un serveur en Rust et des concepts de rendu 3D appliqués comme effets visuels.

## Exigences

- Un serveur de jeu en Rust qui accepte au moins 2 joueurs via UDP.
- Un client Python/Pygame qui se connecte au serveur et effectue le rendu de l'état du jeu.
- Les joueurs peuvent se voir se déplacer en temps réel.
- Au moins une mécanique de jeu : collecter des objets, éviter des obstacles ou un système de combat simple.
- Le jeu possède un écran de démarrage et une condition de fin de partie.
- Le taux d'images est verrouillé à 60 FPS côté client.
- Le projet est commité dans un dépôt Git avec un README expliquant comment le lancer.

## Modèle de départ

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

## Évaluation

Le robot vous demandera :
1. Comment votre serveur gère-t-il deux joueurs qui se déplacent en même temps ?
2. Que se passe-t-il si un joueur se déconnecte ?
3. Pourquoi avez-vous choisi UDP plutôt que TCP ?
4. Comment ajouteriez-vous une troisième dimension à ce jeu ?

## Objectifs supplémentaires

- Ajoutez un tableau des scores qui persiste entre les manches.
- Implémentez la prédiction côté client pour que le mouvement semble instantané.
- Ajoutez un mode spectateur pour les connexions supplémentaires.
