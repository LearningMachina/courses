# Deep Learning

## Konzept

Deep Learning verwendet neuronale Netze mit vielen Schichten, um hierarchische Repräsentationen von Daten zu lernen. Jede Schicht lernt zunehmend abstraktere Merkmale — Pixel → Kanten → Formen → Objekte. Die GPU des Jetson macht es praktikabel, diese Netzwerke direkt auf dem Roboter zu trainieren und auszuführen.

## Themen

- Neuronale Netze: Schichten, Aktivierungen, Backpropagation
- Ein Netzwerk auf den eigenen Sensordaten des Roboters trainieren
- Convolutional Neural Networks (CNNs)
- Wie der YOLO-Detektor des Roboters trainiert wurde
- Transfer Learning: ein vortrainiertes Modell feinabstimmen

## Code

```python
import torch
import torch.nn as nn
from torchvision import models, transforms
from PIL import Image

# --- Einfaches Fully-Connected-Netzwerk für Sensorklassifikation ---
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

# --- Transfer Learning: MobileNetV3 für 2 Klassen feinabstimmen ---
model = models.mobilenet_v3_small(weights="IMAGENET1K_V1")
model.classifier[-1] = nn.Linear(model.classifier[-1].in_features, 2)

# Alle Schichten außer dem Klassifikator einfrieren
for param in model.features.parameters():
    param.requires_grad = False

optimizer = torch.optim.Adam(model.classifier.parameters(), lr=1e-3)
```

```
Backpropagation in einem Satz:
  Berechne den Verlust, gehe dann rückwärts durch das Netzwerk
  und verwende die Kettenregel, um herauszufinden, wie viel jedes Gewicht
  zum Fehler beigetragen hat, und passe dann jedes Gewicht an, um ihn zu reduzieren.
```

## Aktion

1. Baue einen `SensorClassifier`, der 4 Sensormesswerte nimmt und „sicher" oder „Hindernis" ausgibt (2 Klassen).
2. Generiere 500 synthetische Samples (zufällige Eingaben, Label = „Hindernis" wenn ein Wert < 0,3) und trainiere das Netzwerk.
3. Evaluiere auf einem zurückgehaltenen Testset und gib Accuracy, Precision und Recall aus.
4. Lade MobileNetV3 und stimme es auf einem 2-Klassen-Bilddatensatz fein ab, den du von der Roboterkamera gesammelt hast (z. B. „freier Weg" vs „Hindernis").

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was macht eine ReLU-Aktivierung, und warum wird sie anstelle eines Sigmoids in versteckten Schichten verwendet?
- Was ist das Vanishing-Gradient-Problem, und welche Architekturentscheidungen helfen, es zu vermeiden?
- Warum funktioniert Transfer Learning? Was hat das vortrainierte Modell bereits gelernt?

## Erweiterung

Verändere das neuronale Netz, um zu ändern, wie der Roboter seine Welt klassifiziert:

1. Füge eine dritte versteckte Schicht zum `SensorClassifier` hinzu — verbessert sich die Genauigkeit oder overfittet das Modell? Die Entscheidungsgrenze des Roboters wird komplexer.
2. Wechsle die Transfer-Learning-Basis von MobileNetV3 zu einem anderen Modell (z. B. ResNet18) — beobachte, wie sich Klassifikationsgeschwindigkeit und Genauigkeit des Roboters ändern. Architektur beeinflusst Verhalten.
3. Trainiere das Modell auf Bildern von der eigenen Kamera des Roboters statt auf einem Datensatz — der Roboter erkennt jetzt seine eigene Umgebung. Richte ihn auf verschiedene Räume und beobachte, wie sich seine Klassifikationen verschieben.
