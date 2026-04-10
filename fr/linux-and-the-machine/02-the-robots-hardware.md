# Le matériel du robot

## Concept

Votre robot est construit autour du NVIDIA Jetson Orin Nano — un petit ordinateur doté à la fois d'un CPU (pour les tâches générales) et d'un GPU (pour le calcul parallèle rapide, en particulier l'IA). Comprendre les composants physiques vous aide à câbler les choses en toute sécurité et à savoir ce qui est possible avant même d'écrire une seule ligne de code.

## Thèmes

- Qu'est-ce qu'un Jetson Orin Nano ? CPU vs GPU expliqué simplement
- Le boîtier compatible LEGO : comment il fonctionne et ce qu'on peut y fixer
- Ports et connecteurs : USB, GPIO, UART, I2C, SPI, caméra
- La carte de pilotage des moteurs : à quoi elle sert et comment la câbler
- Alimenter le robot : batterie, rails de tension, toujours éteindre avant de câbler
- Votre première interaction : allumer une LED depuis la ligne de commande

## Code

```python
# Blink an LED on GPIO pin 18 using the Jetson GPIO library
import Jetson.GPIO as GPIO
import time

GPIO.setmode(GPIO.BOARD)
GPIO.setup(18, GPIO.OUT, initial=GPIO.LOW)

try:
    while True:
        GPIO.output(18, GPIO.HIGH)
        time.sleep(0.5)
        GPIO.output(18, GPIO.LOW)
        time.sleep(0.5)
finally:
    GPIO.cleanup()
```

```
Jetson Orin Nano — key connectors
──────────────────────────────────
USB-A  ×4    → keyboard, mouse, USB drives
USB-C  ×1    → power input / data
CSI          → ribbon-cable camera (fast, low latency)
GPIO header  → 40-pin: power, ground, digital I/O, UART, I2C, SPI
```

## Action

1. Avec le robot **éteint**, localisez le connecteur GPIO à 40 broches et identifiez la broche 1 (marquée par un triangle ou un pad carré).
2. À l'aide du schéma de brochage, trouvez une broche 3,3 V, une broche GND et une broche GPIO.
3. Connectez une LED (avec une résistance de 330 Ω en série) entre une broche GPIO et GND.
4. Allumez le robot et exécutez le script d'exemple ci-dessus (ajustez le numéro de broche si nécessaire).
5. Vérifiez que la LED clignote, puis faites `Ctrl+C` pour arrêter proprement.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux points suivants :

- Pourquoi faut-il éteindre le robot avant de câbler ?
- Quelle est la différence entre I2C et SPI, et quand choisiriez-vous l'un plutôt que l'autre ?
- Que fait la carte de pilotage des moteurs que les broches GPIO du Jetson ne peuvent pas faire directement ?

## Extension

Modifiez le code pour changer ce que le robot fait physiquement :

1. Changez l'intervalle de clignotement de 1 seconde à 0,1 seconde — observez comment le comportement de la LED passe d'une pulsation calme à un clignotement rapide.
2. Ajoutez une deuxième LED sur une broche GPIO différente et faites-les alterner — le robot dispose maintenant de deux signaux visuels.
3. Créez un motif : trois clignotements rapides suivis d'une pause. Le robot communique maintenant avec un code simple — observez comment modifier le rythme crée du sens.
