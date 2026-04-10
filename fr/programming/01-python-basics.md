# Les bases de Python

## Concept

Python est le langage principal pour le prototypage en robotique : il est lisible, dispose de vastes bibliothèques et fonctionne bien sur le Jetson. Ces fondamentaux — variables, flux de contrôle, fonctions et structures de données — sont les éléments constitutifs de chaque programme que vous écrirez sur le robot.

## Thèmes

- Variables, types de données, opérateurs
- Flux de contrôle : `if`/`else`, boucles `for`, boucles `while`
- Fonctions : définition, appel, paramètres, valeurs de retour
- Listes, dictionnaires, tuples
- Entrées/sorties de fichiers : lecture et écriture de fichiers texte
- Modules et importations : utilisation de la bibliothèque standard
- Introduction aux classes et aux objets
- Gestion des erreurs : `try`/`except`, écrire du code défensif

## Code

```python
# Variables and types
speed = 0.75          # float
name = "LearningMachina"
is_moving = False

# Control flow
if speed > 0:
    print("Robot is moving")
elif speed == 0:
    print("Robot is stopped")
else:
    print("Reversing")

# Function
def clamp(value, low, high):
    """Clamp value to [low, high] range."""
    return max(low, min(value, high))

print(clamp(1.5, 0.0, 1.0))   # 1.0

# List and dict
sensors = [12.3, 11.9, 13.1]
state = {"battery": 87, "temperature": 42.5}

# File I/O
with open("log.txt", "w") as f:
    f.write(f"battery={state['battery']}\n")

# Class
class Motor:
    def __init__(self, name):
        self.name = name
        self.speed = 0.0

    def set_speed(self, speed):
        self.speed = clamp(speed, -1.0, 1.0)

left = Motor("left")
left.set_speed(0.8)
print(f"{left.name}: {left.speed}")

# Error handling
try:
    val = int(input("Enter a number: "))
except ValueError as e:
    print(f"That wasn't a number: {e}")
```

## Action

1. Écrivez une fonction `celsius_to_fahrenheit(c)` et testez-la avec plusieurs valeurs.
2. Créez une classe `Robot` avec `name`, `battery` (int, 0–100) et une méthode `status()` qui affiche une ligne récapitulative.
3. Écrivez une boucle qui lit les lignes d'un fichier et n'affiche que les lignes commençant par `#`.
4. Enveloppez la lecture du fichier dans un bloc `try`/`except` afin qu'un fichier manquant affiche un message d'erreur convivial au lieu d'une trace d'erreur.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Quelle est la différence entre une liste et un dictionnaire ? Donnez un exemple en robotique pour chacun.
- Pourquoi devriez-vous utiliser `with open(...)` plutôt que `f = open(...)` suivi de `f.close()` ?
- Que se passe-t-il lorsqu'une exception est levée et qu'il n'y a pas de bloc `try`/`except` autour ?

## Extension

Modifiez le code pour changer le comportement du robot :

1. Changez la variable `speed` pour une valeur négative — observez comment l'état rapporté du robot passe de « en mouvement » à « en marche arrière ».
2. Ajoutez une méthode `battery_drain()` à la classe `Motor` qui réduit la vitesse de 10 % à chaque appel — le comportement du robot se dégrade désormais au fil du temps.
3. Créez une boucle qui augmente progressivement `left.speed` de 0 à 1.0 — le robot accélère. Puis inversez-la — le robot décélère. Le comportement est désormais une fonction du temps.
