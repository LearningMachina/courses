# Die Linux-Kommandozeile

## Konzept

Ein Betriebssystem ist die Software, die Hardware verwaltet und es dir ermöglicht, Programme auszuführen. Linux ist ein Open-Source-Betriebssystem, das die meisten Server, eingebetteten Systeme und Roboter antreibt — einschließlich deines. Das Terminal ist eine Textschnittstelle zum Betriebssystem: Statt zu klicken, tippst du Befehle ein.

## Themen

- Was ist ein Betriebssystem? Was macht Linux anders?
- Das Terminal: Im Dateisystem navigieren (`cd`, `ls`, `pwd`, `mkdir`, `rm`)
- Dateien lesen und schreiben (`cat`, `nano`, `less`, `echo`, Umleitung)
- Prozesse: Programme starten, in den Hintergrund setzen, beenden (`ps`, `top`, `kill`, `&`)
- Berechtigungen: Was `rwx` bedeutet und warum es wichtig ist
- Paketverwaltung: Software installieren mit `apt`
- Shell-Scripting-Grundlagen: Variablen, Schleifen, if/else in Bash
- SSH: Fernverbindung zum Roboter von einem anderen Rechner

## Code

```bash
# Im Dateisystem navigieren und erkunden
pwd                        # Arbeitsverzeichnis ausgeben
ls -la                     # Dateien mit Berechtigungen auflisten
cd /home/robot             # Verzeichnis wechseln
mkdir my_project           # Ordner erstellen
echo "hello robot" > hi.txt  # Text in eine Datei schreiben
cat hi.txt                 # Datei wieder auslesen

# Einen Prozess im Hintergrund starten
python3 sensor_reader.py &
ps aux | grep python       # Prozess finden
kill 1234                  # Per PID beenden

# Ein einfaches Bash-Skript
#!/bin/bash
for i in 1 2 3; do
  echo "Schleifeniteration $i"
done
```

## Aktion

Öffne ein Terminal auf dem Roboter und:

1. Gib dein aktuelles Verzeichnis mit `pwd` aus.
2. Erstelle einen Ordner namens `practice` in deinem Home-Verzeichnis.
3. Erstelle darin eine Datei namens `hello.txt` mit dem Inhalt `Hallo, Roboter!`.
4. Liste die Datei mit `ls -l` auf und notiere die Berechtigungen.
5. Schreibe ein zweizeiliges Bash-Skript, das das Datum und den aktuellen Benutzer (`whoami`) ausgibt, und führe es aus.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was bewirkt `chmod 755 script.sh`, und warum brauchst du es, bevor du das Skript ausführst?
- Was ist der Unterschied zwischen `>` und `>>` bei der Ausgabeumleitung?
- Warum ist SSH nützlich für die Arbeit mit dem Roboter?

## Erweiterung

Verändere jetzt dein Skript, um das Verhalten des Roboters zu ändern:

1. Ändere die Schleife so, dass sie alle 2 Sekunden ausgibt, statt eine feste Nachricht zu verwenden — beobachte, wie sich der Ausgaberhythmus des Roboters ändert.
2. Füge eine Bedingung hinzu: Wenn die aktuelle Minute gerade ist, gib „Roboter ist wachsam" aus; wenn ungerade, „Roboter ruht sich aus." Beobachte, wie sich die Persönlichkeit des Roboters verschiebt.
3. Erstelle ein zweites Skript, das gleichzeitig mit dem ersten läuft — der Roboter hat jetzt zwei Verhaltensweisen gleichzeitig. Beobachte, was passiert, wenn sie um das Terminal konkurrieren.
