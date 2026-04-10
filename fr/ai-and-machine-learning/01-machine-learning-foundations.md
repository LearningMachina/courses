# Les fondements du Machine Learning

## Concept

Le machine learning est la pratique consistant à écrire des programmes qui s'améliorent grâce à l'expérience plutôt qu'à partir de règles explicites. Au lieu de coder chaque décision, vous montrez des exemples à l'algorithme et il apprend le modèle sous-jacent. Sur le robot, cela signifie lui apprendre à reconnaître des obstacles, interpréter les lectures de capteurs ou prédire les bonnes actions — sans définir manuellement chaque règle.

## Thèmes

- Qu'est-ce que le machine learning ? Apprentissage supervisé, non supervisé et par renforcement
- Régression linéaire et régression logistique à partir de zéro
- Découpage entraînement/validation/test, surapprentissage, régularisation
- Descente de gradient : le moteur de l'apprentissage
- Introduction à PyTorch

## Code

```python
import torch
import torch.nn as nn

# Toy dataset: predict motor speed from distance reading
# (supervised regression)
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
        print(f"Epoch {epoch:3d}  loss={loss.item():.4f}")

# Predict on new input
with torch.no_grad():
    print(model(torch.tensor([[0.4]])))
```

```
Train / Validation / Test split
  Training   70 % — model sees these, updates weights
  Validation 15 % — monitor for overfitting, tune hyperparameters
  Test       15 % — final, one-time evaluation — never peek early!
```

## Action

1. Exécutez l'exemple de régression PyTorch et tracez la droite prédite par rapport aux points de données (utilisez `matplotlib`).
2. Provoquez délibérément un surapprentissage (augmentez le nombre d'époques à 5 000, réduisez le jeu de données à 3 points) et observez la perte de validation augmenter tandis que la perte d'entraînement diminue.
3. Ajoutez une régularisation L2 (`weight_decay=0.01` dans l'optimiseur) et observez comment cela modifie le surapprentissage.
4. Remplacez `nn.Linear` par un réseau à deux couches et comparez les performances.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux points suivants :

- Quelle est la différence entre l'apprentissage supervisé et l'apprentissage par renforcement ? Donnez un exemple robotique pour chacun.
- Que fait concrètement la descente de gradient, étape par étape ?
- Pourquoi est-ce de la triche d'évaluer votre modèle sur le jeu d'entraînement ?

## Extension

Modifiez le modèle pour changer la façon dont le robot apprend :

1. Changez le taux d'apprentissage de 0.01 à 0.1 puis à 0.001 — observez comment la courbe de perte évolue. Trop rapide et le robot dépasse la cible ; trop lent et il apprend à peine.
2. Ajoutez une deuxième variable d'entrée (par ex., la tension de la batterie) au modèle — le robot prend désormais en compte plus de contexte pour prédire la vitesse du moteur. La précision s'améliore-t-elle ?
3. Créez délibérément un scénario de surapprentissage en entraînant pendant 10 000 époques sur 5 points de données — puis testez sur de nouvelles données. Le robot a « mémorisé » au lieu d'« apprendre ». Ajoutez une régularisation et observez la différence.
