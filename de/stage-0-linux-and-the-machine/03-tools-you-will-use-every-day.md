# Werkzeuge für den täglichen Gebrauch

## Konzept

Professionelle Robotik-Entwickler verlassen sich auf eine kleine Auswahl an Werkzeugen für fast alles: einen Texteditor zum Code schreiben, Git zur Änderungsverfolgung und virtuelle Python-Umgebungen, um Abhängigkeiten ordentlich zu halten. Diese Werkzeuge früh zu beherrschen bedeutet, dass du deine Zeit mit Problemlösung verbringst, nicht mit dem Kampf gegen dein eigenes Setup.

## Themen

- Texteditoren: `nano` für schnelle Änderungen, VS Code für echte Projekte
- Git-Grundlagen: `init`, `add`, `commit`, `push`, `pull` — warum Versionskontrolle wichtig ist
- Python-Umgebung: Was eine virtuelle Umgebung ist und warum man sie nutzen sollte
- Dein erstes Python-Skript auf dem Roboter ausführen

## Code

```bash
# --- Git ---
git init my_project        # ein neues Repo starten
cd my_project
echo "# Mein Roboter-Projekt" > README.md
git add README.md
git commit -m "Initial commit"
git remote add origin git@github.com:you/my_project.git
git push -u origin main

# --- Virtuelle Python-Umgebung ---
python3 -m venv .venv          # Umgebung erstellen
source .venv/bin/activate       # aktivieren
pip install flask               # ein Paket in der Umgebung installieren
python3 hello.py                # ein Skript ausführen
deactivate                      # Umgebung verlassen
```

```python
# hello.py — dein erstes Skript auf dem Roboter
import platform

print(f"Hallo von {platform.node()}!")
print(f"Python {platform.python_version()} auf {platform.system()}")
```

## Aktion

1. Erstelle ein neues Git-Repo namens `robot-sandbox` in deinem Home-Verzeichnis.
2. Erstelle darin eine virtuelle Python-Umgebung namens `.venv` und aktiviere sie.
3. Installiere die `requests`-Bibliothek (`pip install requests`).
4. Schreibe ein Skript `hello.py`, das den Hostnamen des Roboters und die Python-Version ausgibt.
5. Füge sowohl `hello.py` als auch eine `.gitignore` hinzu, die `.venv/` ausschließt, und committe beides.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Welches Problem löst eine virtuelle Umgebung? Was geht schief ohne eine?
- Was ist der Unterschied zwischen `git add` und `git commit`?
- Wenn du `hello.py` versehentlich löschst, wie stellst du es mit Git wieder her?

## Erweiterung

Verändere deinen Workflow, um das Entwicklungsverhalten des Roboters zu ändern:

1. Bearbeite deine `hello.py`, um eine andere Nachricht auszugeben, committe die Änderung und nutze dann `git log`, um beide Versionen zu sehen — die Geschichte des Roboters ist jetzt sichtbar.
2. Erstelle einen Branch namens `experiment`, mache dort eine Änderung, wechsle dann zurück zu `main` — beobachte, wie der Code des Roboters in zwei Zuständen gleichzeitig existieren kann.
3. Füge eine zweite Python-Datei hinzu, die aus der ersten importiert — der Code des Roboters hat jetzt Struktur und Abhängigkeiten.
