# Stufe-4-Abschlussmission — Originales autonomes Verhalten

## Ziel

Entwirf, baue und dokumentiere ein originales autonomes Verhalten. Präsentiere es dem Roboter — er wird dich bitten, jede Schicht des Stacks zu erklären.

## Anforderungen

**Verhalten**
- Muss originell sein: keine direkte Kopie eines in den Stufen 0–4 gebauten Verhaltens.
- Muss mindestens drei der fünf Fähigkeitsschichten integrieren:
  1. Motorsteuerung
  2. Sensorwahrnehmung (Entfernung, Vision, Audio oder IMU)
  3. KI-Denken (LLM, Objekterkennung oder trainierter Klassifikator)
  4. Sprachinteraktion (STT und/oder TTS)
  5. Web-Dashboard oder API

**Codequalität**
- Als ROS2-Paket implementiert (oder äquivalente modulare Architektur).
- Mindestens 10 bestandene Unit-Tests, die die Kernlogik abdecken.
- Keine fest codierten Magic Numbers: alle anpassbaren Parameter sind benannte Konstanten oder Konfigurationswerte.

**Dokumentation**
- `README.md` mit: Ziel, Demo-Video-Link, Architekturdiagramm, Einrichtungsanleitung und bekannten Einschränkungen.
- Architekturdiagramm, das jeden Node/jede Komponente und den Datenfluss zwischen ihnen zeigt.
- Kurzer Text (500 Wörter), der Designentscheidungen erklärt und was du beim nächsten Mal anders machen würdest.

**Repository**
- Gesamter Code, Dokumentation und der Datensatz (falls du ein Modell trainiert hast) committed in deinem `robot-sandbox`-Repo unter `graduation-project/`.

## Präsentation vor dem Roboter

Der Roboter wird dir Fragen über alle sechs Stufen stellen. Sei bereit zu erklären:

- **Stufe 0** — Wie du dich per SSH in den Roboter eingeloggt hast und welche Shell-Werkzeuge dir beim Debuggen geholfen haben.
- **Stufe 1** — Welche Datenstrukturen du verwendet hast und warum; wie du Fehler elegant behandelt hast.
- **Stufe 2** — Wie Spieleentwicklungskonzepte (Echtzeit-Schleifen, Zustandsautomaten, Netzwerk) dein Design beeinflusst haben.
- **Stufe 3** — Wie Sensoren gelesen, gefiltert und in die Regelschleife eingespeist werden.
- **Stufe 4** — Welche KI-Modelle du verwendet hast, wie du sie evaluiert hast und was ihre Fehlermodi sind.
- **Stufe 5** — Wie alle Schichten End-to-End verbunden sind; was die Latenz ist; wie du es besser machen würdest.

## Bewertungskriterien

| Kriterium                              | Gewicht |
|----------------------------------------|---------|
| Verhalten funktioniert zuverlässig (Demo) | 30 %  |
| Codequalität und Tests                 | 20 %    |
| Klarheit der Dokumentation             | 20 %    |
| Tiefe der Erklärung an den Roboter     | 20 %    |
| Kreativität und Ehrgeiz               | 10 %    |

## Ideen (wähle eine oder erfinde deine eigene)

- Ein Roboter, der ein einfaches Quizspiel spielt: Fragen laut stellt, Antworten hört, Punkte zählt.
- Ein Sicherheitspatrouille-Bot, der einen Raum kartiert, Eindringlinge erkennt und einen Alarm an das Dashboard sendet.
- Ein Roboter, der farbige Blöcke mit Vision und einem Greifer-Anbau sortiert.
- Ein Roboter-Tutor, der einem anderen Schüler interaktiv die Linux-Befehle aus Stufe 0 beibringt.
- Ein Roboter, der eine Route lernt, indem er einen Menschen einmal fahren sieht, und sie dann autonom wiederholt.

---

*Dieses Projekt abzuschließen bedeutet, dass du einen Roboter gebaut hast, der denkt, sieht, spricht und handelt — vollständig offline, auf Hardware, die in deine Hand passt. Das ist keine Kleinigkeit.*
