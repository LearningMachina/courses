# Comment fonctionne le coach IA *(méta-leçon)*

## Concept

Le coach IA du robot est lui-même un système d'IA — un LLM local augmenté de ce programme pédagogique comme contexte. Comprendre comment il a été construit démystifie la magie et vous montre des patterns réutilisables pour vos propres projets : génération augmentée par récupération (RAG), fine-tuning, pipelines vocaux et boucles d'évaluation.

## Thèmes

- Fine-tuning vs RAG : comment le coach a été construit à partir de ce programme
- Le pipeline vocal : STT → LLM → TTS de bout en bout
- Prompts système et persona : pourquoi le coach répond comme il le fait
- Évaluer un modèle : comment savoir s'il enseigne bien ?
- Comment contribuer une nouvelle leçon pour que le coach l'apprenne aussi

## Code

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

## Action

1. Indexez le répertoire `courses/` dans ChromaDB à l'aide de l'extrait ci-dessus.
2. Interrogez-le avec cinq questions provenant de différentes étapes et évaluez si les passages récupérés sont pertinents.
3. Remplacez le LLM par un autre modèle Ollama et comparez la qualité des réponses.
4. Écrivez un script d'évaluation simple : 10 questions avec des mots-clés attendus ; notez combien de réponses les contiennent.
5. Ajoutez une nouvelle leçon de 100 mots sur un sujet que vous connaissez, indexez-la, et confirmez que le coach peut répondre à des questions à son sujet.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux points suivants :

- Quelle est la différence entre le RAG et le fine-tuning ? Quand utiliseriez-vous chacun ?
- Que fait le prompt système, et que se passe-t-il si vous le supprimez ?
- Comment mesureriez-vous si le coach s'*améliore* dans son enseignement au fil du temps ?

## Extension

Modifiez le système de coaching pour changer la façon dont le robot enseigne :

1. Ajoutez 5 nouveaux paragraphes de leçon sur un sujet que vous maîtrisez et ré-indexez la base de données vectorielle — posez des questions au robot sur votre sujet et observez s'il donne des réponses fondées.
2. Changez le nombre de passages récupérés (top-k) de 3 à 1 puis à 10 — observez comment les réponses du robot changent : trop peu et il devine, trop et il se perd.
3. Changez le modèle LLM utilisé pour la génération (gardez le même pipeline RAG) — le style d'enseignement du robot change tandis que ses connaissances restent les mêmes. Architecture et personnalité sont distinctes.
