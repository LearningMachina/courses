# Mission de l'étape 0 — Script d'état du système

## Objectif

Écrire un script bash qui affiche la température du CPU du robot, le niveau de batterie et un message d'accueil toutes les 5 secondes.

## Exigences

- Le script s'exécute indéfiniment jusqu'à être interrompu avec `Ctrl+C`.
- Toutes les 5 secondes, il affiche :
  - La date et l'heure actuelles
  - La température du CPU (lue depuis `/sys/class/thermal/thermal_zone0/temp` ou `tegrastats`)
  - Le pourcentage de batterie (lu depuis le chemin `/sys/class/power_supply/` approprié)
  - Un court message d'accueil incluant le nom d'hôte du robot
- Le script est exécutable (`chmod +x`) et possède une ligne shebang correcte (`#!/bin/bash`).
- Il est commité dans votre dépôt Git `robot-sandbox`.

## Modèle de départ

```bash
#!/bin/bash

get_cpu_temp() {
  raw=$(cat /sys/class/thermal/thermal_zone0/temp)
  echo "scale=1; $raw / 1000" | bc
}

get_battery() {
  cat /sys/class/power_supply/BAT0/capacity 2>/dev/null || echo "N/A"
}

while true; do
  echo "──────────────────────────────"
  echo "$(date '+%Y-%m-%d %H:%M:%S')"
  echo "CPU temp : $(get_cpu_temp) °C"
  echo "Battery  : $(get_battery) %"
  echo "Hello from $(hostname)!"
  sleep 5
done
```

## Objectifs supplémentaires

- Colorez la température selon sa valeur (vert < 60 °C, jaune < 80 °C, rouge ≥ 80 °C) en utilisant les codes d'échappement ANSI.
- Enregistrez chaque ligne d'état dans un fichier (`status.log`) en plus de l'affichage à l'écran.
- Acceptez un argument en ligne de commande pour modifier l'intervalle (par ex., `./status.sh 10` pour des mises à jour toutes les 10 secondes).

## Réflexion

Après avoir terminé la mission, réfléchissez à ce que vous avez construit :

1. Ce que fait `#!/bin/bash` et pourquoi cela doit être la première ligne.
2. Comment la boucle `while true` fonctionne et comment `sleep 5` contrôle le rythme.
3. Ce qui se passerait si vous retiriez le `2>/dev/null` de la commande de batterie sur une machine sans batterie.
