# LearningMachina — Verkörpertes Curriculum

Ein strukturierter Weg vom absoluten Anfänger zum Erbauer autonomer Roboter.

Im Gegensatz zu traditionellen Kursen trennt LearningMachina nicht zwischen Theorie und Praxis.
**Alles, was der Schüler lernt, steuert direkt den Roboter — und erzeugt sichtbares, physisches Verhalten.**

Der Roboter ist nicht nur ein Werkzeug.
Er ist das **Medium des Lernens**.

Das gesamte Material ist so gestaltet, dass der Roboter es selbst interaktiv unterrichtet —
stelle ihm jederzeit eine Frage, und er wird erklären, demonstrieren und dich anleiten.

---

## Kernprinzip des Lernens

> **Code ist kein abstraktes Konzept — er ist Verhalten.**

Jedes Konzept, das der Schüler lernt, führt zu:
- Bewegung
- Reaktion
- Interaktion
- Entscheidungsfindung

Der Schüler schreibt nicht nur Code.
Er **bringt dem Roboter bei, wie er sich verhalten soll**.

---

## Lektionsformat

Jede Lektion folgt einer einheitlichen, verkörperten Struktur:

1. **Konzept**
   Der Roboter erklärt die Idee in 2–3 einfachen Sätzen.

2. **Code**
   Der Schüler schreibt oder verändert ein kleines Stück Code mit dem SDK des Roboters.

3. **Aktion (Physisches Feedback)**
   Der Roboter führt den Code sofort aus:
   - Bewegt sich
   - Spricht
   - Leuchtet auf
   - Reagiert auf die Umgebung

4. **Reflexion**
   Der Schüler erklärt, was sich im Verhalten des Roboters verändert hat.

5. **Erweiterung**
   Der Schüler verändert den Code, um das Verhalten des Roboters erneut zu ändern.

Alle Lektionen sind einfache Markdown-Dateien. Das LLM des Roboters liest sie als Kontext,
sodass der Schüler jede Folgefrage stellen kann und eine fundierte Antwort erhält.

---

## Das Robot SDK (Kern-Abstraktion)

Alle Programmiersprachen verwenden eine einheitliche, einfache Schnittstelle zur Steuerung des Roboters.

Schüler beginnen nicht mit abstrakten Programmieraufgaben.
Sie beginnen damit, **ein physisches System zu steuern**.

Das ermöglicht:
- Sofortiges Feedback
- Intuitives Verständnis
- Sprachübergreifende Konsistenz (Python, C++, Rust)

---

## Lernmodell

Statt:
> Theorie → Übungen → Tests

verwendet LearningMachina:
> **Verhalten → Verstehen → Kontrolle**

Schüler lernen durch:
- Beobachten, was der Roboter tut
- Code ändern und verschiedene Ergebnisse sehen
- Zunehmend komplexe Verhaltensweisen aufbauen

---

## Verhaltens-Tracks

Parallel zum Hauptfortschritt entwickeln Schüler Fähigkeiten in drei Tracks:

### Bewegung
- Navigation
- Präzise Steuerung
- Sanfte Bewegung

### Interaktion
- Sprachantworten
- Visuelle Erkennung
- Menschliche Interaktion

### Intelligenz
- Entscheidungsfindung
- Lernen aus Daten
- Autonomes Verhalten

---

## Stufe 0 — Umgebung & Erste Steuerung

*Erwecke den Roboter zum Leben.*

Schüler lernen die Grundlagen von Linux, Terminalnutzung, Dateien und Prozessen —
aber jede Interaktion hat ein **physisches Ergebnis**:
- Ein Skript ausführen → Roboter reagiert
- Schleife → wiederholte Bewegung oder Signal
- Befehl → sichtbare Ausgabe am Roboter

> **Stufenziel:**
> Verstehen, dass Befehle auf einem Computer direkt eine Maschine steuern können.

**Die Linux-Kommandozeile**
- Was ist ein Betriebssystem? Was macht Linux anders?
- Das Terminal: Navigation im Dateisystem (cd, ls, pwd, mkdir, rm)
- Dateien lesen und schreiben (cat, nano, less, echo, Umleitung)
- Prozesse: Programme starten, in den Hintergrund setzen, beenden (ps, top, kill, &)
- Berechtigungen: Was rwx bedeutet und warum es wichtig ist
- Paketverwaltung: Software installieren mit apt
- Shell-Scripting-Grundlagen: Variablen, Schleifen, if/else in Bash
- SSH: Fernverbindung zum Roboter von einem anderen Rechner

**Die Hardware des Roboters**
- Was ist ein Jetson Orin Nano? CPU vs GPU einfach erklärt
- Das LEGO-kompatible Gehäuse: Wie es funktioniert und was angebracht werden kann
- Anschlüsse und Verbindungen: USB, GPIO, UART, I2C, SPI, Kamera
- Die Motortreiberplatine: Was sie tut und wie man sie verkabelt
- Stromversorgung des Roboters: Akku, Spannungsschienen, immer ausschalten vor dem Verkabeln
- Erste Interaktion: Eine LED über die Kommandozeile einschalten

**Werkzeuge für den täglichen Gebrauch**
- Texteditoren: nano für schnelle Änderungen, VS Code für echte Projekte
- Git-Grundlagen: init, add, commit, push, pull — warum Versionskontrolle wichtig ist
- Python-Umgebung: Was eine virtuelle Umgebung ist und warum man sie nutzen sollte
- Dein erstes Python-Skript auf dem Roboter ausführen

> **Stufe-0-Mission:** Schreibe ein Bash-Skript, das den Roboter seine
> Vitalwerte melden lässt — CPU-Temperatur, Akkustand und eine gesprochene Begrüßung —
> alle 5 Sekunden. Beobachte, wie der Roboter bei jeder Schleifeniteration zum Leben erwacht.

---

## Stufe 1 — Programmieren als Verhalten

*Lerne, wie ein Computer zu denken — indem du einen Roboter steuerst.*

Jedes Konzept ist an Verhalten gebunden:
- Eine Variable ändern ändert die Bewegung
- Bedingungen erzeugen Reaktionen
- Schleifen erzeugen Muster
- Funktionen erzeugen wiederverwendbare Verhaltensweisen

> **Stufenziel:**
> Verstehen, dass Code definiert, wie sich ein System über die Zeit verhält.

**Python-Grundlagen** (hier starten)
- Variablen, Datentypen, Operatoren
- Kontrollfluss: if/else, for-Schleifen, while-Schleifen
- Funktionen: Definition, Aufruf, Parameter, Rückgabewerte
- Listen, Dictionaries, Tupel
- Datei-I/O: Textdateien lesen und schreiben
- Module und Imports: Die Standardbibliothek nutzen
- Einführung in Klassen und Objekte
- Fehlerbehandlung: try/except, defensiven Code schreiben

**Datenstrukturen & Algorithmen**
- Warum Effizienz auf eingeschränkter Hardware wichtig ist
- Listen vs Dictionaries: Wann man was nutzt
- Stacks, Queues und warum Roboter sie brauchen
- Sortieren und Suchen: Intuition hinter gängigen Algorithmen
- Big-O-Notation: Ein einfaches mentales Modell (keine Mathematik nötig)
- Rekursion: Was es ist, wann es hilft, wann es schadet

**C++-Grundlagen**
- Warum C++? Geschwindigkeit, Speicherkontrolle, Robotik-Industriestandard
- Typen, Pointer, Referenzen
- Kontrollfluss und Funktionen
- Structs und Klassen
- Kompilierung, Linking, Build-Systeme (CMake-Grundlagen)
- Ein einfaches Motorsteuerungsprogramm in C++ schreiben

**Rust-Einführung**
- Warum Rust? Sicherheit ohne Garbage Collection, eingebettete Systeme
- Ownership, Borrowing, Lifetimes
- Fehlerbehandlung mit Result und Option
- Traits und Generics
- Async/Await-Grundlagen
- Wie die eigene Steuerungssoftware des Roboters in Rust geschrieben ist

**Netzwerk & APIs**
- Wie Computer kommunizieren: IP-Adressen, Ports, HTTP-Grundlagen
- Einen einfachen HTTP-Server in Python schreiben (Flask / FastAPI)
- REST-APIs: JSON senden und empfangen
- WebSockets: Echtzeit-Kommunikation zwischen Roboter und Browser
- Ein minimales Web-Dashboard für die Sensordaten deines Roboters bauen
- Node.js in einer Lektion: Wann man JavaScript im Backend nutzt

> **Stufe-1-Mission:** Baue einen Python-Webserver, der die Sensordaten des Roboters
> als Live-JSON-API bereitstellt, abrufbar von jedem Gerät im lokalen Netzwerk.
> Der Zustand des Roboters wird in Echtzeit sichtbar.

---

## Stufe 2 — Robotik & die physische Welt

*Bringe den Roboter dazu, zu fühlen, zu reagieren und sich intelligent zu bewegen.*

Fokus:
- Eingabe (Sensoren) → Ausgabe (Bewegung)
- Interaktion mit der realen Welt
- Regelkreise

Schüler bauen Systeme, in denen der Roboter:
- Hindernisse vermeidet
- Auf seine Umgebung reagiert
- Visuelle Eingaben interpretiert

> **Stufenziel:**
> Verstehen, wie Software mit der physischen Welt interagiert.

**Elektronik-Grundlagen**
- Spannung, Strom, Widerstand — gerade genug, um nichts kaputt zu machen
- Ein Datenblatt lesen: Was die wichtigen Zahlen wirklich bedeuten
- GPIO-Pins: Digitaler Ausgang (an/aus), digitaler Eingang, Pull-up/Pull-down
- Breadboarding: Schaltungen sicher prototypen, bevor man lötet
- Häufige Bauteile: LEDs, Taster, Widerstände, Kondensatoren
- Stromsicherheit: Warum man niemals 5 V anschließt, wo 3,3 V erwartet werden

**Motorsteuerung**
- Wie Gleichstrommotoren und Motortreiber funktionieren
- PWM: Pulsweitenmodulation erklärt
- Differentialantrieb: Wie Kettenfahrzeuge lenken
- PID-Regelung: Bewegung glatt und genau halten
- Eine einfache Fahrschleife in Python schreiben

**Sensoren**
- Sensortypen: Entfernung (Ultraschall, IR, Lidar), IMU, Encoder
- Sensordaten über I2C, UART, SPI lesen
- Verrauschte Sensordaten filtern (gleitender Mittelwert, Kalman-Grundlagen)
- Auf die Umgebung reagieren: Hindernisvermeidung

**Computer Vision**
- Wie Kameras funktionieren: Auflösung, Bildrate, Belichtung
- OpenCV-Grundlagen: Frames aufnehmen, Farbräume, Schwellenwertbildung
- Kantenerkennung, Konturfindung
- Objekterkennung: Was YOLO tut und wie es funktioniert
- Tiefenwahrnehmung: Stereokameras und Disparitätskarten
- Vision auf dem Roboter ausführen: CUDA-Beschleunigung auf Jetson

**Systemintegration**
- ROS2-Grundlagen: Nodes, Topics, Messages
- Eine vollständige Sense-Plan-Act-Schleife schreiben
- Logging, Debugging und Testen auf Hardware

> **Stufe-2-Mission:** Baue einen hindernisausweichenden Roboter, der gleichzeitig
> seinen Kamerafeed an ein Browser-Dashboard streamt, das du in Stufe 1 gebaut hast.
> Beobachte, wie der Roboter in Echtzeit durch seine Umgebung navigiert.

---

## Stufe 3 — KI & Lernsysteme

*Bringe dem Roboter bei, zu denken und sich anzupassen.*

Der Roboter entwickelt sich von:
- Reaktiv → Vorhersagend → Autonom

Schüler bauen Systeme, in denen der Roboter:
- Entscheidungen trifft
- Sprache versteht
- Aus Daten lernt

> **Stufenziel:**
> Verstehen, wie Maschinen lernen und Entscheidungen treffen können.

**Machine-Learning-Grundlagen**
- Was ist Machine Learning? Überwacht vs unüberwacht vs Reinforcement
- Lineare Regression und logistische Regression von Grund auf
- Train/Validation/Test-Splits, Overfitting, Regularisierung
- Gradientenabstieg: Der Motor des Lernens
- Einführung in PyTorch

**Deep Learning**
- Neuronale Netze: Schichten, Aktivierungen, Backpropagation
- Ein Netzwerk auf den eigenen Sensordaten des Roboters trainieren
- Convolutional Neural Networks (CNNs)
- Wie der YOLO-Detektor des Roboters trainiert wurde
- Transfer Learning: Ein vortrainiertes Modell feinabstimmen

**Transformer und Sprachmodelle**
- Attention-Mechanismus: Intuition und Mathematik
- Die Transformer-Architektur
- Large Language Models: GPT, Llama, Gemma, Mistral
- LLMs lokal mit Ollama auf dem Jetson Orin Nano ausführen
- Prompt Engineering und System-Prompts

**Agentische KI**
- Was ist ein KI-Agent? Ziele, Werkzeuge und Feedback-Schleifen
- Function Calling und Tool Use: Einem Modell die Fähigkeit zum Handeln geben
- Einen einfachen Roboter-Agenten bauen: Das LLM entscheidet, welchen Motorbefehl es sendet
- Gedächtnis und Kontext: Wie Agenten sich über Runden hinweg erinnern
- Mehrstufiges Schlussfolgern: Chain-of-Thought und ReAct-Muster
- Sicherheit und Leitplanken: Wann man das Modell nicht entscheiden lassen sollte

**Multimodale Modelle**
- Vision-Language Models (VLMs): Sehen und Verstehen zugleich
- Vision-Language-Action-Modelle (VLAs): Von der Wahrnehmung zur Roboteraktion
- Speech-to-Text (STT): Whisper, Voxtral
- Text-to-Speech (TTS): Piper, Kokoro, ausdrucksstarke Stimmen
- End-to-End-Sprachinteraktionspipelines

**Wie der KI-Coach funktioniert** *(Meta-Lektion)*
- Fine-Tuning vs RAG: Wie der Coach auf diesem Curriculum aufgebaut wurde
- Die Sprachpipeline: STT → LLM → TTS von Anfang bis Ende
- System-Prompts und Persona: Warum der Coach so antwortet, wie er es tut
- Ein Modell evaluieren: Woher weiß man, ob es gut unterrichtet?
- Wie man eine neue Lektion beiträgt, damit der Coach sie ebenfalls lernt

> **Stufe-3-Mission:** Optimiere ein kleines lokales Modell auf ein Thema deiner
> Wahl und füge es der Wissensbasis des Coaches hinzu. Bringe dem Roboter etwas Neues bei.

---

## Stufe 4 — Autonomer Roboter

*Bringe alles zu einem vollständigen System zusammen.*

Schüler entwerfen vollständige Verhaltensweisen:
- Navigation
- Interaktion
- Aufgabenausführung

Der Roboter wird:
- Unabhängig
- Reaktionsfähig
- Zielorientiert

> **Stufenziel:**
> Ein vollständig autonomes System bauen, das in der realen Welt operiert.

**Vollständige Systemarchitektur**
- Wie alle Schichten verbunden sind: Sensoren → Wahrnehmung → Schlussfolgern → Handeln
- Latenz und Echtzeit-Anforderungen
- Alles offline betreiben: Kein Cloud, kein Abonnement
- Profiling und Optimierung: Engpässe auf Jetson-Hardware finden

**Autonome Verhaltensweisen**
- Personenerkennung und Begrüßung
- Sprachgesteuertes Q&A mit dem eingebauten LLM
- Einen Hindernisparcours mit Vision und Motorsteuerung navigieren
- Einer Person mit optischem Fluss folgen
- Auf Sprachbefehle mit physischen Aktionen reagieren

**Eigene Projekte bauen**
- Ein Roboterverhalten von Grund auf entwerfen
- Hardware- und Software-Systeme gemeinsam debuggen
- Dein Projekt dokumentieren und teilen
- Tests für Robotercode schreiben: Warum Hardware-in-the-Loop-Tests wichtig sind

**Zurückgeben**
- Eine neue Lektion zum Curriculum hinzufügen
- Ein eigenes Modell auf eigenen Daten trainieren
- Ein Projekt an die LearningMachina-Community einreichen
- Einen Pull Request öffnen: Wie Open-Source-Zusammenarbeit funktioniert

> **Stufe-4-Abschlussmission:** Entwirf, baue und dokumentiere ein originelles
> autonomes Verhalten. Präsentiere es dem Roboter — er wird dich bitten, jede
> Schicht des Stacks zu erklären.

---

## Missionen statt Übungen

Schüler bearbeiten keine abstrakten Übungen.

Sie erfüllen **Missionen**:
- Einen Raum navigieren
- Einer Person folgen
- Auf Sprachbefehle reagieren
- Auf Umgebungsveränderungen reagieren

Jede Mission kombiniert:
- Programmierung
- Robotik
- KI

---

## Debugging durch Verhalten

Fehler verstecken sich nicht in Logs — sie sind sichtbar:

- Falsche Logik → falsche Bewegung
- Sensorfehler → fehlerhafte Reaktion
- Zustand → sichtbar durch Lichter, Ton, Bewegung

Das macht Debugging:
- Intuitiv
- Sofort
- Physisch

---

## Endergebnis

Am Ende des Curriculums kann der Schüler:

- In mehreren Sprachen programmieren
- Echte Hardware steuern
- Intelligente Systeme bauen
- Autonome Verhaltensweisen entwerfen

Am wichtigsten:

> Sie verstehen, wie man Code in Handlung in der realen Welt verwandelt.

---

## Philosophie-Zusammenfassung

LearningMachina unterrichtet Programmierung nicht als abstrakte Fähigkeit.

Es lehrt:

> **Wie man einer Maschine Verhalten gibt.**
