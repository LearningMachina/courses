# Etwas zurückgeben

## Konzept

Open-Source-Projekte wachsen, weil Mitwirkende Verbesserungen teilen. Das LearningMachina-Curriculum ist dafür gemacht, erweitert zu werden: Jeder Schüler, der ein Thema beherrscht, kann eine Lektion hinzufügen, ein Modell trainieren oder ein Projekt teilen. Lernen, beizutragen, ist selbst eine wertvolle berufliche Fähigkeit — so wird die meiste Robotik-Software gebaut.

## Themen

- Eine neue Lektion zum Curriculum hinzufügen
- Ein benutzerdefiniertes Modell auf eigenen Daten trainieren
- Ein Projekt an die LearningMachina-Community einreichen
- Einen Pull Request öffnen: wie Open-Source-Zusammenarbeit funktioniert

## Code

```bash
# Fork → Branch → Commit → PR Workflow
git clone git@github.com:DEIN_USERNAME/LearningMachina.courses.git
cd courses
git checkout -b lesson/add-kalman-filter

# Deine Lektionsdatei erstellen
cp template.md stage-2-robotics/06-kalman-filter.md
# ... bearbeiten ...

git add stage-2-robotics/06-kalman-filter.md
git commit -m "Kalman-Filter-Lektion zu Stufe 2 Sensoren hinzufügen"
git push origin lesson/add-kalman-filter

# Einen Pull Request auf GitHub öffnen
gh pr create --title "Kalman-Filter-Lektion hinzufügen" \
             --body "Behandelt den 1D-Kalman-Filter mit einem ausgearbeiteten Sensorbeispiel."
```

```markdown
<!-- Lektionsvorlage (jede neue Lektion muss diesem Format folgen) -->
# Lektionstitel

## Konzept
Zwei bis drei Sätze, die die Idee erklären.

## Themen
- Aufzählungsliste der behandelten Unterthemen

## Code
\`\`\`python
# Ausführbarer Code mit dem SDK des Roboters
\`\`\`

## Aktion
Führe den Code aus und beobachte die physische Reaktion des Roboters.

## Reflexion
Erkläre, was sich im Verhalten des Roboters geändert hat.

## Erweiterung
Verändere den Code, um das Verhalten des Roboters erneut zu ändern.
```

## Aktion

1. Forke das `courses`-Repository und klone deinen Fork.
2. Schreibe eine neue Lektion zu einem Thema, das noch nicht abgedeckt ist (z. B. SLAM, Verhaltensbäume, ROS2-Actions).
3. Folge der Lektionsvorlage genau.
4. Öffne einen Pull Request mit einem klaren Titel und einer Beschreibung.
5. Reagiere auf alle Review-Kommentare der Maintainer.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist der Unterschied zwischen einem Fork und einem Branch?
- Warum bitten Open-Source-Projekte Mitwirkende, einen Pull Request zu öffnen, statt direkt auf `main` zu pushen?
- Was macht eine gute Commit-Nachricht aus, und warum ist sie in einem kollaborativen Projekt wichtig?

## Erweiterung

Verändere deinen Beitrag, um die Community und das Curriculum zu stärken:

1. Schreibe eine zweite Lektion zu einem verwandten Thema und verlinke sie mit der ersten — das Curriculum wächst, und das Coaching-Wissen des Roboters erweitert sich.
2. Bitte einen anderen Schüler (oder den Roboter), deine Lektion zu reviewen und das Feedback einzuarbeiten — der Unterricht des Roboters verbessert sich durch Zusammenarbeit.
3. Trainiere den RAG-Coach auf deiner neuen Lektion und teste ihn mit 5 Fragen — wenn der Roboter dein Thema korrekt lehren kann, hast du seinen Geist erfolgreich erweitert.
