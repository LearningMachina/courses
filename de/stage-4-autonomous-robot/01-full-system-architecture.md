# Vollständige Systemarchitektur

## Konzept

Ein autonomer Roboter ist ein geschichtetes System: Rohe Sensordaten fließen durch Wahrnehmungs- und Denkschichten nach oben, und Befehle fließen zurück nach unten zu den Aktoren. Jede Schicht führt Latenz ein, daher ist es essenziell zu verstehen, wo Zeit verbracht wird — und was die Echtzeit-Einschränkungen sind — bevor man versucht zu optimieren.

## Themen

- Wie alle Schichten verbunden sind: Sensoren → Wahrnehmung → Denken → Aktion
- Latenz und Echtzeit-Einschränkungen
- Alles offline betreiben: kein Cloud, kein Abo
- Profiling und Optimierung: Engpässe auf Jetson-Hardware finden

## Code

```
Vollständiger Stack auf einem Jetson Orin Nano
────────────────────────────────────────────────────────
Schicht         Komponente              Typische Latenz
────────────────────────────────────────────────────────
Sensoren        Ultraschall, IMU        1–10 ms
Wahrnehmung     OpenCV, YOLO            30–80 ms (GPU)
Sprachmodell    Ollama / llama.cpp      200–800 ms
Motorsteuerung  ROS2-Node → PWM         1–5 ms
TTS / STT       Piper / Whisper-tiny    100–400 ms
────────────────────────────────────────────────────────
Gesamter Roundtrip (Sprachbefehl → Aktion): ~500 ms–1,5 s
```

```python
# Eine Pipeline-Stufe mit Pythons cProfile profilieren
import cProfile, pstats, io

def perception_pipeline(frame):
    # ... dein OpenCV-/YOLO-Code ...
    pass

pr = cProfile.Profile()
pr.enable()
perception_pipeline(frame)
pr.disable()

s = io.StringIO()
pstats.Stats(pr, stream=s).sort_stats("cumulative").print_stats(10)
print(s.getvalue())
```

```bash
# Jetson-Systemüberwachung
tegrastats                      # GPU / CPU / Speicher / Leistung live
jtop                            # Komfortables TUI (pip install jetson-stats)
sudo nvpmodel -m 0              # Maximale-Leistung-Modus
sudo jetson_clocks              # Taktraten auf Maximum sperren
```

## Aktion

1. Zeichne ein Blockdiagramm deines gesamten Roboter-Stacks, beschrifte jede Komponente und die Daten, die zwischen ihnen fließen.
2. Nutze `tegrastats` während YOLO läuft und notiere GPU-Auslastung, Speicherverbrauch und Stromverbrauch.
3. Profiliere die Wahrnehmungspipeline mit `cProfile` und identifiziere die drei teuersten Funktionsaufrufe.
4. Wechsle zwischen Jetson-Leistungsmodi (`nvpmodel`) und miss die Änderung der YOLO-Inferenzzeit.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was bedeutet „Echtzeit" im Robotik-Kontext, und welche Schichten deines Stacks müssen harte Echtzeit-Anforderungen erfüllen?
- Warum ist es wichtig, offline ohne Cloud-Abhängigkeit zu laufen? Nenne zwei Szenarien, in denen Cloud-Abhängigkeit versagen würde.
- Was ist der Unterschied zwischen Latenz und Durchsatz, und was ist wichtiger für Hindernisvermeidung vs Sprachinteraktion?

## Erweiterung

Verändere die Systemarchitektur, um die Leistungseigenschaften des Roboters zu ändern:

1. Wechsle den Jetson-Leistungsmodus mit `nvpmodel` und starte deinen YOLO-Benchmark erneut — beobachte, wie der Roboter Stromverbrauch gegen Geschwindigkeit eintauscht. Im Niedrigenergiemodus sieht er die Welt in Zeitlupe.
2. Füge einen Zeitstempel zu jeder Schicht hinzu (Sensormessung, Wahrnehmung, Denken, Motorbefehl) und berechne die End-to-End-Latenz — der Roboter weiß jetzt, wie schnell er denkt. Identifiziere den Engpass und versuche ihn zu verkleinern.
3. Deaktiviere eine Schicht (z. B. überspringe das LLM-Denken) und beobachte, wie das Verhalten des Roboters zu rein reaktiver Steuerung degradiert — zu verstehen, was jede Schicht beiträgt, macht das Gesamtsystem klarer.
