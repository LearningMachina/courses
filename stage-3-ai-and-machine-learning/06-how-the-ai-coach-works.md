# How the AI Coach Works *(meta-lesson)*

## Concept

The robot's AI coach is itself an AI system — a local LLM augmented with this curriculum as context. Understanding how it was built demystifies the magic and shows you patterns you can reuse for your own projects: retrieval-augmented generation (RAG), fine-tuning, voice pipelines, and evaluation loops.

## Topics

- Fine-tuning vs RAG: how the coach was built on this curriculum
- The voice pipeline: STT → LLM → TTS end to end
- System prompts and persona: why the coach answers the way it does
- Evaluating a model: how do you know if it's teaching well?
- How to contribute a new lesson so the coach learns it too

## Example

```
RAG pipeline (what the coach uses):

  Student asks a question
        │
        ▼
  Embed the question  (sentence-transformers)
        │
        ▼
  Vector search over curriculum chunks  (ChromaDB / FAISS)
        │
        ▼
  Top-k relevant lesson passages selected
        │
        ▼
  Injected into LLM context  (Ollama / llama.cpp)
        │
        ▼
  LLM generates grounded answer
        │
        ▼
  TTS speaks the answer  (Piper / Kokoro)
```

```python
# Minimal RAG example
from sentence_transformers import SentenceTransformer
import chromadb, ollama

encoder = SentenceTransformer("all-MiniLM-L6-v2")
client  = chromadb.Client()
col     = client.create_collection("curriculum")

# Index lessons (run once)
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
            {"role": "system", "content": f"You are the LearningMachina coach.\n\nContext:\n{context}"},
            {"role": "user",   "content": question},
        ],
    )
    return response["message"]["content"]

print(ask_coach("What is PWM and why does the robot use it?"))
```

## Exercise

1. Index the `courses/` directory into ChromaDB using the snippet above.
2. Query it with five questions from different stages and evaluate whether the retrieved passages are relevant.
3. Swap the LLM for a different Ollama model and compare answer quality.
4. Write a simple evaluation script: 10 questions with expected keywords; score how many answers contain them.
5. Add a new 100-word lesson about a topic you know, index it, and confirm the coach can answer questions about it.

## Check

Explain to the robot:

- What is the difference between RAG and fine-tuning? When would you use each?
- What does the system prompt do, and what happens if you remove it?
- How would you measure whether the coach is getting *better* at teaching over time?
