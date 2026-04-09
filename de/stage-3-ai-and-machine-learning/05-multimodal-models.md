# Multimodale Modelle

## Konzept

Multimodale Modelle verarbeiten mehr als eine Art von Eingabe — Text, Bilder, Audio oder alle drei gleichzeitig. Vision-Language-Modelle lassen den Roboter sehen und beschreiben, was vor ihm ist. Sprache hinzuzufügen verwandelt ihn in einen natürlichen Sprachassistenten, der vollständig offline auf Jetson-Hardware läuft.

## Themen

- Vision-Language-Modelle (VLMs): gleichzeitig sehen und verstehen
- Vision-Language-Action-Modelle (VLAs): von der Wahrnehmung zur Roboteraktion
- Speech-to-Text (STT): Whisper, Voxtral
- Text-to-Speech (TTS): Piper, Kokoro, ausdrucksstarke Stimmen
- End-to-End-Sprachinteraktionspipelines

## Code

```python
# VLM: beschreiben, was die Kamera sieht (mit Ollama und llava)
import ollama, base64, cv2

cap = cv2.VideoCapture(0)
ret, frame = cap.read()
cap.release()

_, buffer = cv2.imencode(".jpg", frame)
image_b64 = base64.b64encode(buffer).decode()

response = ollama.chat(
    model="llava:7b",
    messages=[
        {
            "role": "user",
            "content": "Beschreibe, was du siehst. Gibt es Hindernisse?",
            "images": [image_b64],
        }
    ],
)
print(response["message"]["content"])
```

```python
# STT: Mikrofoneingabe mit faster-whisper transkribieren
from faster_whisper import WhisperModel
import sounddevice as sd, numpy as np

model = WhisperModel("tiny", device="cuda", compute_type="int8")

audio = sd.rec(int(3 * 16000), samplerate=16000, channels=1, dtype="float32")
sd.wait()

segments, _ = model.transcribe(audio.flatten(), language="de")
text = " ".join(s.text for s in segments)
print("Du hast gesagt:", text)
```

```python
# TTS: eine Antwort mit Piper synthetisieren
import subprocess, tempfile, os

def speak(text: str):
    with tempfile.NamedTemporaryFile(suffix=".wav", delete=False) as f:
        wav_path = f.name
    subprocess.run(
        ["piper", "--model", "de_DE-thorsten-medium.onnx", "--output_file", wav_path],
        input=text.encode(), check=True
    )
    subprocess.run(["aplay", wav_path])
    os.unlink(wav_path)

speak("Hindernis erkannt. Drehe nach rechts.")
```

## Aktion

1. Lade `llava:7b` mit Ollama herunter und beschreibe ein Foto von der Kamera deines Roboters.
2. Nimm 3 Sekunden Sprache auf, transkribiere sie mit Whisper `tiny` und gib das Ergebnis aus.
3. Synthetisiere einen kurzen Satz mit Piper und spiele ihn über den Lautsprecher des Roboters ab.
4. Verkette alle drei: Zuhören → Transkribieren → VLM fragen, was es sieht → Antwort laut sprechen.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist der Unterschied zwischen einem VLM und einem VLA, und warum ist ein VLA schwerer zu bauen?
- Warum STT und TTS lokal ausführen, statt eine Cloud-API zu nutzen?
- Was ist der Latenzflaschenhals in der Zuhören-→-Denken-→-Sprechen-Pipeline auf Jetson-Hardware?

## Erweiterung

Verändere die multimodale Pipeline, um zu ändern, wie der Roboter wahrnimmt und kommuniziert:

1. Ändere den VLM-Prompt von „Beschreibe dieses Bild" zu „Welche Hindernisse siehst du?" — das visuelle Verständnis des Roboters wird aufgabenspezifisch. Gleiche Kamera, andere Interpretation.
2. Wechsle die TTS-Stimme oder Sprechgeschwindigkeit — die Persönlichkeit des Roboters ändert sich hörbar. Eine langsame, tiefe Stimme wirkt ruhig; eine schnelle, hohe Stimme wirkt dringend.
3. Verkette die volle Pipeline: der Roboter hört zu → transkribiert → schaut in die Kamera → beschreibt, was er sieht → spricht die Antwort. Frage ihn „Was ist vor dir?" und erlebe eine vollständige multimodale Interaktion.
