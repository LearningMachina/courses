# Les bases de l'électronique

## Concept

Avant de connecter quoi que ce soit au robot, vous avez besoin d'un modèle mental de l'électricité : la tension est la pression, le courant est le flux et la résistance limite le flux. Se tromper peut détruire une broche GPIO, voire le Jetson — mais bien comprendre ces notions ouvre un monde de capteurs et d'actionneurs.

## Thèmes

- Tension, courant, résistance — juste assez pour ne rien casser
- Lire une fiche technique : ce que les chiffres importants signifient réellement
- Broches GPIO : sortie numérique (on/off), entrée numérique, pull-up/pull-down
- Platine d'essai : prototyper des circuits en toute sécurité avant de souder
- Composants courants : LEDs, boutons, résistances, condensateurs
- Sécurité électrique : pourquoi il ne faut jamais connecter du 5 V là où du 3,3 V est attendu

## Code

```
Ohm's Law:  V = I × R

LED circuit:
  3.3 V ──[330 Ω]──[LED]── GND

Current = 3.3 V / 330 Ω ≈ 10 mA   ✓ safe for GPIO
```

```python
# Reading a button with internal pull-up
import Jetson.GPIO as GPIO

BUTTON_PIN = 15
GPIO.setmode(GPIO.BOARD)
GPIO.setup(BUTTON_PIN, GPIO.IN, pull_up_down=GPIO.PUD_UP)

while True:
    if GPIO.input(BUTTON_PIN) == GPIO.LOW:   # pressed (pulled to GND)
        print("Button pressed!")
```

```
Datasheet key numbers to check:
  VCC / VDD  — supply voltage (must match your rail: 3.3 V or 5 V)
  VIH / VIL  — logic HIGH/LOW thresholds for inputs
  IOH / IOL  — max current sourced/sunk by GPIO
  Absolute Maximum Ratings — never exceed these
```

## Action

1. Calculez la bonne valeur de résistance pour une LED (Vf = 2,0 V) connectée à 3,3 V, en visant 10 mA.
2. Câblez la LED avec cette résistance sur une platine d'essai et testez-la avec le script GPIO de l'étape 0.
3. Ajoutez un bouton sur la platine d'essai et écrivez un script qui allume la LED tant que le bouton est maintenu enfoncé.
4. Trouvez la fiche technique du Jetson Orin Nano et repérez la spécification de courant maximum des GPIO.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Que se passe-t-il si vous connectez la sortie d'un capteur 5 V directement à une entrée GPIO 3,3 V du Jetson ?
- Que fait une résistance pull-up, et pourquoi est-elle nécessaire pour un bouton ?
- Pourquoi un condensateur est-il utile à proximité de l'alimentation d'un pilote de moteur ?

## Extension

Modifiez le circuit pour changer les signaux physiques du robot :

1. Remplacez la résistance de 330 Ω par une résistance de 1 kΩ — la LED faiblit. Le signal du robot est maintenant plus discret. Essayez 100 Ω (avec prudence) — il devient plus fort.
2. Ajoutez une seconde LED d'une couleur différente sur une autre broche GPIO et faites-les signaler différents états (vert = sûr, rouge = obstacle détecté) — le robot communique désormais par la couleur.
3. Câblez un bouton et modifiez le code pour que la LED ne s'allume que lorsqu'il est pressé — le robot réagit maintenant au toucher humain. C'est votre première boucle entrée → sortie.
