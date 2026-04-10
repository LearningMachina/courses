# Modèles multimodaux

## Concept

Les modèles multimodaux traitent plus d'un type d'entrée — texte, images, audio, ou les trois à la fois. Les modèles vision-langage (VLM) permettent au robot de voir et de décrire ce qui se trouve devant lui. L'ajout de la parole en fait un assistant vocal naturel fonctionnant entièrement hors ligne sur le matériel Jetson.

## Thèmes

- Modèles vision-langage (VLM) : voir et comprendre simultanément
- Modèles vision-langage-action (VLA) : de la perception à l'action robotique
- Reconnaissance vocale (STT) : Whisper, Voxtral
- Synthèse vocale (TTS) : Piper, Kokoro, voix expressives
- Pipelines d'interaction vocale de bout en bout

## Code

```python
# VLM: describe what the camera sees (using Ollama with llava)
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
            "content": "Describe what you see. Are there any obstacles?",
            "images": [image_b64],
        }
    ],
)
print(response["message"]["content"])
```

```python
# STT: transcribe microphone input with faster-whisper
from faster_whisper import WhisperModel
import sounddevice as sd, numpy as np

model = WhisperModel("tiny", device="cuda", compute_type="int8")

audio = sd.rec(int(3 * 16000), samplerate=16000, channels=1, dtype="float32")
sd.wait()

segments, _ = model.transcribe(audio.flatten(), language="en")
text = " ".join(s.text for s in segments)
print("You said:", text)
```

```python
# TTS: synthesise a response with Piper
import subprocess, tempfile, os

def speak(text: str):
    with tempfile.NamedTemporaryFile(suffix=".wav", delete=False) as f:
        wav_path = f.name
    subprocess.run(
        ["piper", "--model", "en_US-lessac-medium.onnx", "--output_file", wav_path],
        input=text.encode(), check=True
    )
    subprocess.run(["aplay", wav_path])
    os.unlink(wav_path)

speak("Obstacle detected. Turning right.")
```

## Action

1. Téléchargez `llava:7b` avec Ollama et décrivez une photo prise par la caméra de votre robot.
2. Enregistrez 3 secondes de parole, transcrivez-les avec Whisper `tiny` et affichez le résultat.
3. Synthétisez une courte phrase avec Piper et jouez-la via le haut-parleur du robot.
4. Chaînez les trois étapes : écouter → transcrire → demander au VLM ce qu'il voit → prononcer la réponse à voix haute.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux points suivants :

- Quelle est la différence entre un VLM et un VLA, et pourquoi un VLA est-il plus difficile à construire ?
- Pourquoi exécuter le STT et le TTS localement plutôt que d'utiliser une API cloud ?
- Quel est le goulot d'étranglement de latence dans le pipeline écouter → réfléchir → parler sur le matériel Jetson ?

## Extension

Modifiez le pipeline multimodal pour changer la façon dont le robot perçoit et communique :

1. Changez le prompt du VLM de « Décrivez cette image » à « Quels obstacles voyez-vous ? » — la compréhension visuelle du robot devient spécifique à la tâche. Même caméra, interprétation différente.
2. Changez la voix ou le débit du TTS — la personnalité du robot change de manière audible. Une voix lente et grave semble calme ; une voix rapide et aiguë semble urgente.
3. Chaînez le pipeline complet : le robot écoute → transcrit → regarde la caméra → décrit ce qu'il voit → prononce la réponse. Demandez-lui « Qu'est-ce qu'il y a devant toi ? » et vivez une interaction multimodale complète.
