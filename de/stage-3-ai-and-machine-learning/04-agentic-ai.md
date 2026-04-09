# Agentische KI

## Konzept

Ein KI-Agent ist ein System, das seine Umgebung wahrnimmt, über Ziele nachdenkt und Aktionen durchführt — möglicherweise Werkzeuge aufruft, Code ausführt oder Hardware steuert. Einem LLM die Fähigkeit zu geben, Funktionen aufzurufen, verwandelt es von einem Textgenerator in einen Akteur, der die Welt verändern kann — einschließlich deines Roboters.

## Themen

- Was ist ein KI-Agent? Ziele, Werkzeuge und Feedbackschleifen
- Funktionsaufruf und Werkzeugnutzung: einem Modell die Fähigkeit zu handeln geben
- Einen einfachen Roboter-Agenten bauen: das LLM entscheidet, welchen Motorbefehl es sendet
- Gedächtnis und Kontext: wie Agenten sich über Turns hinweg erinnern
- Mehrstufiges Denken: Chain-of-Thought und ReAct-Muster
- Sicherheit und Leitplanken: wann man das Modell nicht entscheiden lassen sollte

## Code

```python
import ollama, json

# Werkzeuge definieren, die der Agent aufrufen darf
tools = [
    {
        "type": "function",
        "function": {
            "name": "drive",
            "description": "Fahrrichtung und Geschwindigkeit des Roboters setzen.",
            "parameters": {
                "type": "object",
                "properties": {
                    "direction": {"type": "string", "enum": ["forward", "backward", "left", "right", "stop"]},
                    "speed":     {"type": "number", "description": "0.0 bis 1.0"},
                },
                "required": ["direction", "speed"],
            },
        },
    }
]

def drive(direction: str, speed: float):
    print(f"[MOTOR] {direction} @ {speed:.2f}")
    # TODO: in tatsächliche PWM-Befehle übersetzen

def agent_turn(sensor_summary: str):
    messages = [
        {"role": "system", "content": "Du steuerst einen Kettenroboter. Nutze das drive-Werkzeug, um auf Sensordaten zu reagieren."},
        {"role": "user",   "content": sensor_summary},
    ]
    response = ollama.chat(model="llama3.2:1b", messages=messages, tools=tools)

    for tool_call in response.get("message", {}).get("tool_calls", []):
        name = tool_call["function"]["name"]
        args = tool_call["function"]["arguments"]
        if name == "drive":
            drive(**args)

# Einen Agenten-Turn ausführen
agent_turn("Frontdistanz: 0,25 m. Linke Distanz: 0,8 m. Rechte Distanz: 1,2 m.")
```

```
ReAct-Muster: Denken → Handeln → Beobachten → Denken → …
  Gedanke: Das Hindernis ist vorne und links nah. Ich sollte nach rechts drehen.
  Aktion:  drive(direction="right", speed=0.5)
  Beobachtung: Frontdistanz ist jetzt 0,9 m.
  Gedanke: Weg ist frei. Vorwärts fahren.
  Aktion:  drive(direction="forward", speed=0.6)
```

## Aktion

1. Führe das Agenten-Beispiel mit einem lokalen Ollama-Modell aus und beobachte seine Werkzeugaufrufe.
2. Füge ein zweites Werkzeug `speak(text)` hinzu, das den Roboter etwas sagen lässt, und bitte den Agenten, seine Aktionen zu kommentieren.
3. Implementiere eine dreistufige ReAct-Schleife: Wahrnehmen → Agent entscheidet → Handeln → erneut Wahrnehmen → Agent entscheidet → Handeln.
4. Füge eine Sicherheitsleitplanke hinzu: Wenn der Agent versucht, `speed > 0.8` zu setzen, begrenze es auf `0.8` und logge eine Warnung.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist der Unterschied zwischen einem einzelnen LLM-Aufruf und einer agentischen Schleife?
- Warum sind Leitplanken wichtig, wenn ein LLM physische Hardware steuert?
- Was ist Chain-of-Thought-Prompting, und wie verbessert es das Denken bei komplexen Aufgaben?

## Erweiterung

Verändere den Agenten, um die autonome Entscheidungsfindung des Roboters zu ändern:

1. Füge ein `look_around()`-Werkzeug hinzu, das den Roboter 360° dreht und berichtet, was YOLO erkennt — der Roboter kann jetzt Informationen sammeln, bevor er entscheidet, was zu tun ist.
2. Wechsle den Agenten von Single-Turn zu einer 5-Turn-ReAct-Schleife — beobachte, wie das Verhalten des Roboters überlegter wird: er denkt, handelt, beobachtet und passt sich an.
3. Füge eine Leitplanke hinzu, die das Fahren verhindert, wenn der Akku unter 20 % ist — der Roboter hat jetzt Selbsterhaltung. Entferne die Leitplanke und beobachte: der Roboter fährt, bis er stirbt. Sicherheit muss explizit sein.
