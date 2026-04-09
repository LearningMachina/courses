# Grundlagen des Maschinellen Lernens

## Konzept

Maschinelles Lernen ist die Praxis, Programme zu schreiben, die sich durch Erfahrung verbessern, statt durch explizite Regeln. Anstatt jede Entscheidung zu programmieren, zeigst du dem Algorithmus Beispiele und er lernt das Muster. Auf dem Roboter bedeutet das, ihm beizubringen, Hindernisse zu erkennen, Sensordaten zu interpretieren oder gute Aktionen vorherzusagen — ohne jede Regel von Hand zu erstellen.

## Themen

- Was ist maschinelles Lernen? Überwacht vs unüberwacht vs Reinforcement
- Lineare Regression und logistische Regression von Grund auf
- Train/Validation/Test-Splits, Overfitting, Regularisierung
- Gradientenabstieg: der Motor des Lernens
- Einführung in PyTorch

## Code

```python
import torch
import torch.nn as nn

# Spielzeugdatensatz: Motorgeschwindigkeit aus Entfernungsmessung vorhersagen
# (überwachte Regression)
X = torch.tensor([[1.0], [0.5], [0.3], [0.1], [0.8]], dtype=torch.float32)
y = torch.tensor([[0.2], [0.4], [0.6], [1.0], [0.3]], dtype=torch.float32)

model     = nn.Linear(1, 1)
criterion = nn.MSELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)

for epoch in range(200):
    pred = model(X)
    loss = criterion(pred, y)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    if epoch % 50 == 0:
        print(f"Epoche {epoch:3d}  Loss={loss.item():.4f}")

# Auf neuer Eingabe vorhersagen
with torch.no_grad():
    print(model(torch.tensor([[0.4]])))
```

```
Train / Validation / Test Split
  Training   70 % — das Modell sieht diese, aktualisiert Gewichte
  Validation 15 % — Overfitting beobachten, Hyperparameter tunen
  Test       15 % — finale, einmalige Auswertung — niemals vorher hinschauen!
```

## Aktion

1. Führe das PyTorch-Regressionsbeispiel aus und plotte die vorhergesagte Linie gegen die Datenpunkte (nutze `matplotlib`).
2. Bringe das Modell absichtlich zum Overfitten (Epochen auf 5.000 erhöhen, Datensatz auf 3 Punkte reduzieren) und beobachte, wie der Validierungsverlust steigt, während der Trainingsverlust fällt.
3. Füge L2-Regularisierung hinzu (`weight_decay=0.01` im Optimizer) und sieh, wie sie das Overfitting verändert.
4. Ersetze `nn.Linear` durch ein zweischichtiges Netzwerk und vergleiche die Leistung.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist der Unterschied zwischen überwachtem Lernen und Reinforcement Learning? Gib jeweils ein Roboterbeispiel.
- Was macht Gradientenabstieg tatsächlich, Schritt für Schritt?
- Warum ist es Betrug, dein Modell auf dem Trainingsset zu evaluieren?

## Erweiterung

Verändere das Modell, um zu ändern, wie der Roboter lernt:

1. Ändere die Lernrate von 0,01 auf 0,1 und dann 0,001 — beobachte, wie sich die Verlustkurve verändert. Zu schnell und der Roboter schießt über; zu langsam und er lernt kaum.
2. Füge ein zweites Eingabefeature hinzu (z. B. Batteriespannung) zum Modell — der Roboter berücksichtigt jetzt mehr Kontext bei der Vorhersage der Motorgeschwindigkeit. Verbessert sich die Genauigkeit?
3. Erzeuge absichtlich ein Overfitting-Szenario, indem du 10.000 Epochen auf 5 Datenpunkten trainierst — teste dann auf neuen Daten. Der Roboter hat „auswendig gelernt" statt „gelernt". Füge Regularisierung hinzu und beobachte den Unterschied.
