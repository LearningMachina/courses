# La ligne de commande Linux

## Concept

Un système d'exploitation est le logiciel qui gère le matériel et vous permet d'exécuter des programmes. Linux est un système d'exploitation open source qui fait tourner la plupart des serveurs, des systèmes embarqués et des robots — y compris le vôtre. Le terminal est une interface textuelle vers le système d'exploitation : au lieu de cliquer, vous tapez des commandes.

## Thèmes

- Qu'est-ce qu'un système d'exploitation ? Qu'est-ce qui rend Linux différent ?
- Le terminal : naviguer dans le système de fichiers (`cd`, `ls`, `pwd`, `mkdir`, `rm`)
- Lire et écrire des fichiers (`cat`, `nano`, `less`, `echo`, redirection)
- Les processus : exécuter des programmes, les mettre en arrière-plan, les arrêter (`ps`, `top`, `kill`, `&`)
- Les permissions : ce que signifie `rwx` et pourquoi c'est important
- La gestion des paquets : installer des logiciels avec `apt`
- Les bases du scripting shell : variables, boucles, if/else en bash
- SSH : se connecter au robot à distance depuis une autre machine

## Code

```bash
# Navigate and explore the filesystem
pwd                        # print working directory
ls -la                     # list files with permissions
cd /home/robot             # change directory
mkdir my_project           # create a folder
echo "hello robot" > hi.txt  # write text to a file
cat hi.txt                 # read it back

# Run a process in the background
python3 sensor_reader.py &
ps aux | grep python       # find it
kill 1234                  # stop it by PID

# A simple bash script
#!/bin/bash
for i in 1 2 3; do
  echo "Loop iteration $i"
done
```

## Action

Ouvrez un terminal sur le robot et :

1. Affichez votre répertoire courant avec `pwd`.
2. Créez un dossier appelé `practice` dans votre répertoire personnel.
3. À l'intérieur, créez un fichier appelé `hello.txt` contenant le texte `Hello, robot!`.
4. Listez le fichier avec `ls -l` et notez les permissions.
5. Écrivez un script bash de deux lignes qui affiche la date et l'utilisateur courant (`whoami`), puis exécutez-le.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux points suivants :

- Que fait `chmod 755 script.sh`, et pourquoi en avez-vous besoin avant d'exécuter le script ?
- Quelle est la différence entre `>` et `>>` lors de la redirection de la sortie ?
- Pourquoi SSH est-il utile pour travailler avec le robot ?

## Extension

Modifiez maintenant votre script pour changer le comportement du robot :

1. Modifiez la boucle pour afficher toutes les 2 secondes au lieu d'utiliser un message fixe — observez comment le rythme de sortie du robot change.
2. Ajoutez une condition : si la minute actuelle est paire, affichez « Le robot est en alerte » ; si elle est impaire, affichez « Le robot se repose. » Observez le changement de personnalité du robot.
3. Créez un second script qui s'exécute en parallèle du premier — le robot a maintenant deux comportements qui tournent en même temps. Observez ce qui se passe quand ils se disputent le terminal.
