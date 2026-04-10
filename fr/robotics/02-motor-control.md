# Contrôle des moteurs

## Concept

Les moteurs à courant continu tournent à une vitesse proportionnelle à la tension qui leur est appliquée. Un pilote de moteur est une puce située entre les broches GPIO à faible courant du Jetson et les fils du moteur à fort courant, traduisant les signaux PWM en puissance réelle. Pour un robot à chenilles, la transmission différentielle — en faisant varier la vitesse de chaque côté — produit tous les mouvements de virage.

## Thèmes

- Fonctionnement des moteurs à courant continu et des pilotes de moteurs
- PWM : la modulation de largeur d'impulsion expliquée
- Transmission différentielle : comment les robots à chenilles dirigent
- Contrôle PID : maintenir un mouvement fluide et précis
- Écrire une boucle de conduite simple en Python

## Code

```
PWM: a square wave whose duty cycle sets the average voltage.
  100% duty ≈ full speed forward
   50% duty ≈ half speed
    0% duty ≈ stopped
```

```python
# Simple differential drive with RPi.GPIO-compatible PWM
import Jetson.GPIO as GPIO
import time

LEFT_PWM_PIN  = 32
RIGHT_PWM_PIN = 33
FREQ_HZ = 1000

GPIO.setmode(GPIO.BOARD)
GPIO.setup(LEFT_PWM_PIN,  GPIO.OUT)
GPIO.setup(RIGHT_PWM_PIN, GPIO.OUT)

left  = GPIO.PWM(LEFT_PWM_PIN,  FREQ_HZ)
right = GPIO.PWM(RIGHT_PWM_PIN, FREQ_HZ)
left.start(0)
right.start(0)

def drive(left_pct, right_pct):
    """Set motor speeds as percentages (0–100)."""
    left.ChangeDutyCycle(max(0, min(100, left_pct)))
    right.ChangeDutyCycle(max(0, min(100, right_pct)))

drive(70, 70)   # straight forward
time.sleep(2)
drive(70, 30)   # turn right
time.sleep(1)
drive(0, 0)     # stop

left.stop()
right.stop()
GPIO.cleanup()
```

```python
# Minimal PID controller
class PID:
    def __init__(self, kp, ki, kd):
        self.kp, self.ki, self.kd = kp, ki, kd
        self._integral = 0.0
        self._prev_error = 0.0

    def update(self, error, dt):
        self._integral += error * dt
        derivative = (error - self._prev_error) / dt if dt > 0 else 0
        self._prev_error = error
        return self.kp * error + self.ki * self._integral + self.kd * derivative
```

## Action

1. Câblez votre pilote de moteur au Jetson conformément à sa fiche technique.
2. Exécutez l'exemple `drive()` et confirmez les comportements avant / virage / arrêt.
3. Implémentez une fonction `go_straight(duration)` qui fait avancer le robot pendant le nombre de secondes donné puis s'arrête.
4. Ajoutez une boucle PID qui maintient les deux roues à la même vitesse en utilisant le retour des encodeurs (ou simulez-le avec du bruit aléatoire).

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Qu'est-ce que le rapport cyclique, et comment un rapport cyclique de 50 % à 1 kHz se traduit-il en tension moyenne ?
- Pourquoi un robot à chenilles tourne-t-il à droite lorsque le moteur gauche va plus vite ?
- Que corrige chacun des trois termes PID (P, I, D) ?

## Extension

Modifiez le code des moteurs pour changer la façon dont le robot se déplace :

1. Changez le rapport cyclique PWM de 50 % à 25 % puis à 75 % — observez physiquement la différence de vitesse. Le robot rampe, puis fonce.
2. Modifiez les constantes PID : doublez Kp et observez l'oscillation, puis augmentez Kd pour l'amortir — la qualité du mouvement du robot change visiblement.
3. Écrivez une séquence qui avance pendant 2 secondes, tourne à 90° (une chenille en avant, l'autre en arrière), puis avance à nouveau — le robot parcourt un chemin en L. Le timing et la vitesse créent la géométrie.
