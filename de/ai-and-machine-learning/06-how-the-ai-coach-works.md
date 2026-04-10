# Wie der KI-Coach funktioniert *(Meta-Lektion)*

## Konzept

Der KI-Coach des Roboters ist selbst ein KI-System — ein lokales LLM, erweitert mit diesem Curriculum als Kontext. Zu verstehen, wie er gebaut wurde, entmystifiziert die Magie und zeigt dir Muster, die du für deine eigenen Projekte wiederverwenden kannst: Retrieval-Augmented Generation (RAG), Feinabstimmung, Sprachpipelines und Evaluierungsschleifen.

## Themen

- Feinabstimmung vs RAG: wie der Coach auf diesem Curriculum aufgebaut wurde
- Die Sprachpipeline: STT → LLM → TTS End-to-End
- System-Prompts und Persona: warum der Coach so antwortet, wie er es tut
- Ein Modell evaluieren: woher weißt du, ob es gut unterrichtet?
- Wie man eine neue Lektion beiträgt, damit der Coach sie auch lernt

## Code

```
RAG-Pipeline (die der Coach verwendet):

  Schüler stellt eine Frage
        │
        ▼
  Frage einbetten  (sentence-transformers)
        │
        ▼
  Vektorsuche über Curriculum-Chunks  (ChromaDB / FAISS)
        │
        ▼
  Top-k relevante Lektionspassagen ausgewählt
        │
        ▼
  In LLM-Kontext eingefügt  (Ollama / llama.cpp)
        │
        ▼
  LLM generiert fundierte Antwort
        │
        ▼
  TTS spricht die Antwort  (Piper / Kokoro)
```

```python
# Minimales RAG-Beispiel
from sentence_transformers import SentenceTransformer
import chromadb, ollama

encoder = SentenceTransformer("all-MiniLM-L6-v2")
client  = chromadb.Client()
col     = client.create_collection("curriculum")

# Lektionen indizieren (einmal ausführen)
import pathlib
for md in pathlib.Path("courses").rglob("*.md"):
    text = md.read_text()
    col.add(documents=[text], ids=[str(md)])

def ask_coach(question: str) -> str:
    q_emb   = encoder.encode([question]).tolist()
    results = col.query(query_embeddings=q_emb, n_results=3)
    context = "\n\n".join(results["documents"][0])

    response = ollama.chat(
        model="gemma:2b",
        messages=[
            {"role": "system", "content": f"Du bist der LearningMachina-Coach.\n\nKontext:\n{context}"},
            {"role": "user",   "content": question},
        ],
    )
    return response["message"]["content"]

print(ask_coach("Was ist PWM und warum nutzt der Roboter es?"))
```

## Aktion

1. Indiziere das `courses/`-Verzeichnis in ChromaDB mit dem obigen Snippet.
2. Stelle fünf Fragen aus verschiedenen Stufen und bewerte, ob die abgerufenen Passagen relevant sind.
3. Tausche das LLM gegen ein anderes Ollama-Modell aus und vergleiche die Antwortqualität.
4. Schreibe ein einfaches Evaluierungsskript: 10 Fragen mit erwarteten Schlüsselwörtern; bewerte, wie viele Antworten sie enthalten.
5. Füge eine neue 100-Wörter-Lektion über ein Thema hinzu, das du kennst, indiziere sie und bestätige, dass der Coach Fragen dazu beantworten kann.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist der Unterschied zwischen RAG und Feinabstimmung? Wann würdest du was verwenden?
- Was macht der System-Prompt, und was passiert, wenn du ihn entfernst?
- Wie würdest du messen, ob der Coach im Laufe der Zeit *besser* im Unterrichten wird?

## Erweiterung

Verändere das Coaching-System, um zu ändern, wie der Roboter unterrichtet:

1. Füge 5 neue Lektionsabschnitte zu einem Thema hinzu, das du gut kennst, und indiziere die Vektordatenbank neu — frage den Roboter nach deinem Thema und beobachte, ob er fundierte Antworten gibt.
2. Ändere die Anzahl der abgerufenen Passagen (top-k) von 3 auf 1 und dann 10 — beobachte, wie sich die Antworten des Roboters ändern: zu wenige und er rät, zu viele und er wird verwirrt.
3. Tausche das für die Generierung verwendete LLM-Modell aus (behalte die gleiche RAG-Pipeline) — der Lehrstil des Roboters ändert sich, während sein Wissen gleich bleibt. Architektur und Persönlichkeit sind getrennt.
