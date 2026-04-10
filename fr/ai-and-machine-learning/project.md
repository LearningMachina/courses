# Mission de l'étape 3 — Affiner un modèle local

## Objectif

Affinez (fine-tune) un petit modèle local sur un sujet de votre choix et ajoutez-le à la base de connaissances du coach.

## Exigences

**Jeu de données**
- Choisissez un sujet lié à la robotique, à l'électronique ou à la programmation.
- Créez un jeu de données d'au moins 50 paires question-réponse dans le format utilisé pour le fine-tuning par instructions (par ex., format Alpaca ou ShareGPT).
- Divisez-le en un ensemble d'entraînement (80 %) et un ensemble de validation (20 %).

**Fine-tuning**
- Utilisez `unsloth`, `llama.cpp` fine-tune, ou HuggingFace `transformers` + `trl` pour affiner un modèle ≤ 3 milliards de paramètres.
- Entraînez sur le Jetson ou sur un ordinateur portable / instance cloud et transférez les poids.
- Sauvegardez le checkpoint résultant au format GGUF ou safetensors.

**Intégration**
- Ajoutez le modèle affiné comme modèle de coach alternatif dans votre pipeline RAG de la méta-leçon.
- Démontrez qu'il répond correctement à cinq questions sur le sujet choisi.

**Documentation**
- Rédigez un court `README.md` décrivant : votre sujet, le processus de création du jeu de données, la configuration d'entraînement et les résultats.
- Committez le tout dans le répertoire `stage3-project/` de votre dépôt `robot-sandbox`.

## Objectifs supplémentaires

- Comparez le modèle de base et le modèle affiné sur votre ensemble de validation à l'aide d'un script de notation automatisé.
- Publiez votre jeu de données sur Hugging Face Hub.
- Ajoutez votre nouveau fichier Markdown de leçon dans `courses/` et indexez-le pour que le coach RAG puisse également répondre aux questions à son sujet.

## Réflexion

Présentez votre modèle affiné au robot et expliquez :

1. Ce que le fine-tuning a concrètement changé dans les poids du modèle, à un niveau conceptuel.
2. Comment vous avez veillé à ce que votre jeu de données soit suffisamment diversifié pour éviter le surapprentissage sur un style de formulation restreint.
3. Un cas d'échec que vous avez observé et ce que vous feriez pour le corriger.
