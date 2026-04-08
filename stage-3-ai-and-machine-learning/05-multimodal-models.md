# Multimodal Models

## Concept

Multimodal models process more than one type of input — text, images, audio, or all three at once. Vision-Language Models let the robot see and describe what is in front of it. Adding speech turns it into a natural voice assistant that runs entirely offline on Jetson hardware.

## Topics

- Vision-Language Models (VLMs): seeing and understanding at once
- Vision-Language-Action models (VLAs): from perception to robot action
- Speech-to-Text (STT): Whisper, Voxtral
- Text-to-Speech (TTS): Piper, Kokoro, expressive voices
- End-to-end voice interaction pipelines

## Example

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

## Exercise

1. Pull `llava:7b` with Ollama and describe a photograph from your robot's camera.
2. Record 3 seconds of speech, transcribe it with Whisper `tiny`, and print the result.
3. Synthesise a short sentence with Piper and play it through the robot's speaker.
4. Chain all three: listen → transcribe → ask VLM what it sees → speak the answer aloud.

## Check

Explain to the robot:

- What is the difference between a VLM and a VLA, and why is a VLA harder to build?
- Why run STT and TTS locally rather than using a cloud API?
- What is the latency bottleneck in the listen → think → speak pipeline on Jetson hardware?
