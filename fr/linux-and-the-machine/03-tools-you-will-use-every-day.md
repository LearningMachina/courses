# Les outils que vous utiliserez chaque jour

## Concept

Les développeurs professionnels en robotique s'appuient sur un petit ensemble d'outils pour presque tout : un éditeur de texte pour écrire du code, Git pour suivre les modifications, et les environnements virtuels Python pour garder les dépendances bien organisées. Maîtriser ces outils tôt signifie que vous passerez votre temps à résoudre des problèmes, et non à vous battre avec votre propre configuration.

## Thèmes

- Éditeurs de texte : `nano` pour les modifications rapides, VS Code pour les vrais projets
- Les bases de Git : `init`, `add`, `commit`, `push`, `pull` — pourquoi le contrôle de version est important
- Environnement Python : qu'est-ce qu'un environnement virtuel et pourquoi en utiliser un
- Exécuter votre premier script Python sur le robot

## Code

```bash
# --- Git ---
git init my_project        # start a new repo
cd my_project
echo "# My Robot Project" > README.md
git add README.md
git commit -m "Initial commit"
git remote add origin git@github.com:you/my_project.git
git push -u origin main

# --- Python virtual environment ---
python3 -m venv .venv          # create the environment
source .venv/bin/activate       # activate it
pip install flask               # install a package inside the env
python3 hello.py                # run a script
deactivate                      # leave the environment
```

```python
# hello.py — your first script on the robot
import platform

print(f"Hello from {platform.node()}!")
print(f"Python {platform.python_version()} on {platform.system()}")
```

## Action

1. Créez un nouveau dépôt Git appelé `robot-sandbox` dans votre répertoire personnel.
2. À l'intérieur, créez un environnement virtuel Python appelé `.venv` et activez-le.
3. Installez la bibliothèque `requests` (`pip install requests`).
4. Écrivez un script `hello.py` qui affiche le nom d'hôte du robot et la version de Python.
5. Ajoutez et commitez à la fois `hello.py` et un `.gitignore` qui exclut `.venv/`.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux points suivants :

- Quel problème un environnement virtuel résout-il ? Que se passe-t-il sans ?
- Quelle est la différence entre `git add` et `git commit` ?
- Si vous supprimez accidentellement `hello.py`, comment le récupérer avec Git ?

## Extension

Modifiez votre flux de travail pour changer le comportement de développement du robot :

1. Modifiez votre `hello.py` pour afficher un message différent, commitez le changement, puis utilisez `git log` pour voir les deux versions — l'historique du robot est maintenant visible.
2. Créez une branche appelée `experiment`, faites-y une modification, puis revenez sur `main` — observez comment le code du robot peut exister dans deux états à la fois.
3. Ajoutez un second fichier Python qui importe depuis le premier — le code du robot a maintenant une structure et des dépendances.
