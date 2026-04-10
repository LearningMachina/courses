# Jeux 2D avec Python

## Concept

Les jeux sont des systèmes en temps réel — tout comme les robots. Une boucle de jeu lit les entrées, met à jour l'état et effectue le rendu à chaque image. Python avec Pygame vous offre le chemin le plus simple pour comprendre la programmation en temps réel : sprites, détection de collisions, physique et animation — le tout visible à l'écran.

## Thèmes

- Qu'est-ce qu'une boucle de jeu ? Entrée → Mise à jour → Rendu
- Les bases de Pygame : initialiser une fenêtre, gérer les événements, dessiner des formes
- Sprites et images : chargement, positionnement, animation
- Détection de collisions : boîtes englobantes, pixel parfait
- Physique simple : gravité, vélocité, accélération
- Effets sonores et musique
- Cartes de tuiles : construire des niveaux à partir de grilles
- Gestion de l'état du jeu : menus, en cours de jeu, fin de partie

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

1. Installez Pygame : `pip install pygame`.
2. Créez un fichier `jump_game.py` avec le code ci-dessus.
3. Exécutez-le et appuyez sur Espace pour faire sauter le carré.
4. Ajoutez un second rectangle comme obstacle qui se déplace de droite à gauche.
5. Détectez quand le joueur entre en collision avec l'obstacle et affichez `"Game Over!"`.

## Réflexion

- Que se passe-t-il si vous changez la valeur de la gravité ? La hauteur du saut ?
- En quoi la boucle de jeu est-elle similaire à la boucle perception-planification-action d'un robot ?
- Pourquoi le jeu tourne-t-il à 60 FPS ? Que se passe-t-il si vous supprimez `clock.tick(60)` ?

## Extension

- Ajoutez un compteur de score qui augmente chaque seconde où le joueur survit.
- Ajoutez des images de sprites au lieu de simples rectangles.
- Créez un effet de particules simple lorsque le joueur atterrit.
