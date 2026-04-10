# Contribuer en retour

## Concept

Les projets open-source grandissent parce que les contributeurs partagent leurs améliorations. Le programme LearningMachina est conçu pour être étendu : tout étudiant qui maîtrise un sujet peut ajouter une leçon, entraîner un modèle ou partager un projet. Apprendre à contribuer est en soi une compétence professionnelle précieuse — c'est ainsi que la plupart des logiciels robotiques sont construits.

## Thèmes

- Ajouter une nouvelle leçon au programme
- Entraîner un modèle personnalisé sur vos propres données
- Soumettre un projet à la communauté LearningMachina
- Ouvrir un pull request : comment fonctionne la collaboration open-source

## Code

```bash
# Fork → branch → commit → PR workflow
git clone git@github.com:YOUR_USERNAME/LearningMachina.courses.git
cd courses
git checkout -b lesson/add-kalman-filter

# Create your lesson file
cp template.md robotics/06-kalman-filter.md
# ... edit it ...

git add robotics/06-kalman-filter.md
git commit -m "Add Kalman filter lesson to Stage 3 Sensors"
git push origin lesson/add-kalman-filter

# Open a pull request on GitHub
gh pr create --title "Add Kalman filter lesson" \
             --body "Covers 1D Kalman filter with a worked sensor example."
```

```markdown
<!-- Lesson template (every new lesson must follow this) -->
# Lesson Title

## Concept
Two to three sentences explaining the idea.

## Topics
- Bullet list of sub-topics covered

## Code
\`\`\`python
# Runnable code using the robot's SDK
\`\`\`

## Action
Run the code and observe the robot's physical response.

## Reflection
Explain what changed in the robot's behavior.

## Extension
Modify the code to change the robot's behavior again.
```

## Action

1. Faites un fork du dépôt `courses` et clonez votre fork.
2. Rédigez une nouvelle leçon sur un sujet non encore couvert (par ex., SLAM, arbres de comportement, actions ROS2).
3. Suivez exactement le modèle de leçon.
4. Ouvrez un pull request avec un titre et une description clairs.
5. Répondez à tout commentaire de revue des mainteneurs.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez à :

- Quelle est la différence entre un fork et une branche ?
- Pourquoi les projets open-source demandent-ils aux contributeurs d'ouvrir un pull request plutôt que de pousser directement sur `main` ?
- Qu'est-ce qui fait un bon message de commit, et pourquoi est-ce important dans un projet collaboratif ?

## Extension

Modifiez votre contribution pour renforcer la communauté et le programme :

1. Rédigez une seconde leçon sur un sujet connexe et liez-la à la première — le programme s'enrichit, et les connaissances de coaching du robot s'élargissent.
2. Demandez à un autre étudiant (ou au robot) de réviser votre leçon et intégrez ses retours — l'enseignement du robot s'améliore grâce à la collaboration.
3. Entraînez le coach RAG sur votre nouvelle leçon et testez-le avec 5 questions — si le robot peut enseigner votre sujet avec précision, vous avez réussi à étendre son esprit.
