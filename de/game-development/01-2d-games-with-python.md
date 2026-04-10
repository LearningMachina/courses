# 2D-Spiele mit Python

## Konzept

Spiele sind Echtzeitsysteme — genau wie Roboter. Eine Spielschleife liest Eingaben, aktualisiert den Zustand und rendert die Ausgabe in jedem Frame. Python mit Pygame bietet den einfachsten Weg, Echtzeitprogrammierung zu verstehen: Sprites, Kollisionserkennung, Physik und Animation — alles sichtbar auf dem Bildschirm.

## Themen

- Was ist eine Spielschleife? Eingabe → Aktualisierung → Darstellung
- Pygame-Grundlagen: Fenster initialisieren, Events verarbeiten, Formen zeichnen
- Sprites und Bilder: laden, positionieren, animieren
- Kollisionserkennung: Bounding Boxes, pixelgenau
- Einfache Physik: Schwerkraft, Geschwindigkeit, Beschleunigung
- Soundeffekte und Musik
- Tile Maps: Level aus Rastern aufbauen
- Spielzustandsverwaltung: Menüs, Spielen, Game-Over

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

## Aktion

1. Installiere Pygame: `pip install pygame`.
2. Erstelle eine Datei `jump_game.py` mit dem obigen Code.
3. Führe es aus und drücke die Leertaste, um das Quadrat springen zu lassen.
4. Füge ein zweites Rechteck als Hindernis hinzu, das sich von rechts nach links bewegt.
5. Erkenne, wenn der Spieler mit dem Hindernis kollidiert, und gib `"Game Over!"` aus.

## Reflexion

- Was passiert, wenn du den Schwerkraftwert änderst? Die Sprunghöhe?
- Wie ähnelt die Spielschleife der Sense-Plan-Act-Schleife eines Roboters?
- Warum läuft das Spiel mit 60 FPS? Was passiert, wenn du `clock.tick(60)` entfernst?

## Erweiterung

- Füge einen Punktezähler hinzu, der jede Sekunde steigt, die der Spieler überlebt.
- Füge Sprite-Bilder anstelle von einfachen Rechtecken hinzu.
- Erstelle einen einfachen Partikeleffekt, wenn der Spieler landet.
