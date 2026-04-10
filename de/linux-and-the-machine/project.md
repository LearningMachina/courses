# Stufe-0-Mission — Systemstatus-Skript

## Ziel

Schreibe ein Bash-Skript, das die CPU-Temperatur, den Akkustand und eine Begrüßungsnachricht des Roboters alle 5 Sekunden ausgibt.

## Anforderungen

- Das Skript läuft unbegrenzt, bis es mit `Ctrl+C` unterbrochen wird.
- Alle 5 Sekunden gibt es aus:
  - Aktuelles Datum und Uhrzeit
  - CPU-Temperatur (gelesen aus `/sys/class/thermal/thermal_zone0/temp` oder `tegrastats`)
  - Akkustand in Prozent (gelesen aus dem entsprechenden `/sys/class/power_supply/`-Pfad)
  - Eine kurze Begrüßung, die den Hostnamen des Roboters enthält
- Das Skript ist ausführbar (`chmod +x`) und hat eine korrekte Shebang-Zeile (`#!/bin/bash`).
- Es ist in dein `robot-sandbox`-Git-Repository committed.

## Starter-Vorlage

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
  echo "CPU-Temp : $(get_cpu_temp) °C"
  echo "Akku     : $(get_battery) %"
  echo "Hallo von $(hostname)!"
  sleep 5
done
```

## Zusatzaufgaben

- Farbcodierung der Temperatur (grün < 60 °C, gelb < 80 °C, rot ≥ 80 °C) mit ANSI-Escape-Codes.
- Jede Statuszeile zusätzlich zur Bildschirmausgabe in eine Datei (`status.log`) loggen.
- Ein Kommandozeilenargument akzeptieren, um das Intervall zu ändern (z. B. `./status.sh 10` für 10-Sekunden-Updates).

## Reflexion

Nachdem du die Mission abgeschlossen hast, reflektiere über das, was du gebaut hast:

1. Was `#!/bin/bash` bewirkt und warum es die erste Zeile sein muss.
2. Wie die `while true`-Schleife funktioniert und wie `sleep 5` das Timing steuert.
3. Was passieren würde, wenn du `2>/dev/null` aus dem Akku-Befehl auf einer Maschine ohne Akku entfernst.
