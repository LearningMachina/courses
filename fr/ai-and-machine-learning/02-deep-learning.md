# Deep Learning

## Concept

Le deep learning utilise des réseaux de neurones à nombreuses couches pour apprendre des représentations hiérarchiques des données. Chaque couche apprend des caractéristiques de plus en plus abstraites — pixels → contours → formes → objets. Le GPU du Jetson permet d'entraîner et d'exécuter ces réseaux directement sur le robot.

## Thèmes

- Réseaux de neurones : couches, fonctions d'activation, rétropropagation
- Entraîner un réseau sur les propres données de capteurs du robot
- Réseaux de neurones convolutifs (CNN)
- Comment le détecteur YOLO du robot a été entraîné
- Transfer learning : affiner un modèle pré-entraîné

## Code

```python
import torch
import torch.nn as nn
from torchvision import models, transforms
from PIL import Image

# --- Simple fully-connected network for sensor classification ---
class SensorClassifier(nn.Module):
    def __init__(self, input_size, num_classes):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(input_size, 64),
            nn.ReLU(),
            nn.Linear(64, 32),
            nn.ReLU(),
            nn.Linear(32, num_classes),
        )

    def forward(self, x):
        return self.net(x)

# --- Transfer learning: fine-tune MobileNetV3 for 2 classes ---
model = models.mobilenet_v3_small(weights="IMAGENET1K_V1")
model.classifier[-1] = nn.Linear(model.classifier[-1].in_features, 2)

# Freeze all layers except the classifier
for param in model.features.parameters():
    param.requires_grad = False

optimizer = torch.optim.Adam(model.classifier.parameters(), lr=1e-3)
```

```
Backpropagation in one sentence:
  Compute the loss, then walk backwards through the network
  using the chain rule to find how much each weight contributed
  to the error, then nudge each weight to reduce it.
```

## Action

1. Construisez un `SensorClassifier` qui prend 4 lectures de capteurs en entrée et produit « sûr » ou « obstacle » (2 classes).
2. Générez 500 échantillons synthétiques (entrées aléatoires, étiquette = « obstacle » si une lecture < 0.3) et entraînez le réseau.
3. Évaluez sur un jeu de test séparé et affichez l'exactitude, la précision et le rappel.
4. Chargez MobileNetV3 et affinez-le sur un jeu d'images à 2 classes collectées depuis la caméra du robot (par ex., « chemin dégagé » vs « obstacle »).

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux points suivants :

- Que fait une activation ReLU, et pourquoi est-elle utilisée à la place d'une sigmoïde dans les couches cachées ?
- Qu'est-ce que le problème du gradient qui s'évanouit, et quels choix architecturaux permettent de l'éviter ?
- Pourquoi le transfer learning fonctionne-t-il ? Qu'a déjà appris le modèle pré-entraîné ?

## Extension

Modifiez le réseau de neurones pour changer la façon dont le robot classifie son environnement :

1. Ajoutez une troisième couche cachée au `SensorClassifier` — la précision s'améliore-t-elle ou le modèle surapprend-il ? La frontière de décision du robot devient plus complexe.
2. Changez la base du transfer learning de MobileNetV3 à un autre modèle (par ex., ResNet18) — observez comment la vitesse de classification et la précision du robot évoluent. L'architecture influence le comportement.
3. Entraînez le modèle sur des images provenant de la propre caméra du robot plutôt que d'un jeu de données externe — le robot reconnaît désormais son propre environnement. Pointez-le vers différentes pièces et observez ses classifications changer.
