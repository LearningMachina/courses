# Transformer und Sprachmodelle

## Konzept

Die Transformer-Architektur — aufgebaut auf dem Attention-Mechanismus — veränderte die KI 2017. Heute treibt sie jedes große Sprachmodell an: GPT, Llama, Gemma, Mistral. Auf dem Jetson Orin Nano kannst du 3–7-B-Parameter-Modelle lokal ausführen und deinem Roboter ein Gehirn geben, das denken, erklären und sich unterhalten kann, ohne Cloud-Verbindung.

## Themen

- Attention-Mechanismus: Intuition und Mathematik
- Die Transformer-Architektur
- Large Language Models: GPT, Llama, Gemma, Mistral
- LLMs lokal mit Ollama auf Jetson Orin Nano ausführen
- Prompt Engineering und System-Prompts

## Code

```bash
# Ollama installieren und ein Modell laden, das klein genug für den Jetson ist
curl -fsSL https://ollama.com/install.sh | sh
ollama pull gemma:2b          # ~1,5 GB, läuft gut auf Jetson Orin Nano
ollama run gemma:2b           # interaktiver Chat

# Oder aus Python aufrufen
pip install ollama
```

```python
import ollama

response = ollama.chat(
    model="gemma:2b",
    messages=[
        {
            "role": "system",
            "content": (
                "Du bist die Onboard-KI eines kleinen Kettenroboters namens LearningMachina. "
                "Antworte knapp. Du kannst den Roboter steuern, indem du JSON-Befehle ausgibst."
            ),
        },
        {"role": "user", "content": "Was kannst du vor dir sehen?"},
    ],
)
print(response["message"]["content"])
```

```
Attention — Intuition
  Für jedes Token fragt das Modell:
    „Welche anderen Tokens in dieser Sequenz sind für mich am relevantesten?"
  Es vergibt Gewichte (Attention Scores) und bildet eine gewichtete Summe.
  Multi-Head Attention macht das mehrfach parallel,
  wobei jeder Head lernt, auf verschiedene Arten von Beziehungen zu achten.
```

## Aktion

1. Installiere Ollama, lade `gemma:2b` oder `llama3.2:1b` herunter und führe ein Gespräch von der Kommandozeile.
2. Schreibe ein Python-Skript, das die aktuellen Sensordaten des Roboters (als String formatiert) an das Modell sendet und es entscheiden lässt: vorwärts, links-drehen, rechts-drehen oder stoppen.
3. Experimentiere mit drei verschiedenen System-Prompts und notiere, wie sie den Ton und Stil des Modells verändern.
4. Miss den Tokens-pro-Sekunde-Durchsatz auf dem Jetson mit und ohne GPU-Offloading.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was bedeutet „Kontextfenster", und was passiert, wenn du es überschreitest?
- Was ist der Unterschied zwischen einem Basismodell und einem instruction-tuned Modell?
- Warum erlaubt Quantisierung (z. B. Q4_K_M) ein größeres Modell auf dem Jetson, und was ist der Kompromiss?

## Erweiterung

Verändere das LLM-Setup, um zu ändern, wie der Roboter denkt und spricht:

1. Ändere den System-Prompt von einem hilfreichen Assistenten zu einem vorsichtigen Sicherheitsbeauftragten — stelle die gleiche Frage und beobachte, wie sich Persönlichkeit und Entscheidungen des Roboters komplett ändern.
2. Wechsle von `gemma:2b` zu einem größeren Modell (wenn der Speicher reicht) oder einem kleineren — miss den Speed-vs-Qualität-Kompromiss. Der Roboter wird entweder klüger-aber-langsamer oder schneller-aber-einfacher.
3. Füttere den Roboter mit Live-Sensordaten im Prompt und bitte ihn, seine Erfahrungen zu erzählen — der Roboter beschreibt jetzt seinen eigenen Zustand in natürlicher Sprache. Er hat eine Stimme.
