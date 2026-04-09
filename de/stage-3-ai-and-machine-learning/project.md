# Stufe-3-Mission — Ein lokales Modell feinabstimmen

## Ziel

Stimme ein kleines lokales Modell auf ein Thema deiner Wahl fein ab und füge es zur Wissensbasis des Coaches hinzu.

## Anforderungen

**Datensatz**
- Wähle ein Thema mit Bezug zu Robotik, Elektronik oder Programmierung.
- Erstelle einen Datensatz mit mindestens 50 Frage-Antwort-Paaren im Format für Instruction Fine-Tuning (z. B. Alpaca- oder ShareGPT-Format).
- Teile in Trainings- (80 %) und Validierungssets (20 %) auf.

**Feinabstimmung**
- Nutze `unsloth`, `llama.cpp` Fine-Tune oder HuggingFace `transformers` + `trl`, um ein Modell ≤ 3 B Parameter feinabzustimmen.
- Trainiere auf dem Jetson oder auf einem Laptop/Cloud-Instanz und übertrage die Gewichte.
- Speichere den resultierenden GGUF- oder Safetensors-Checkpoint.

**Integration**
- Füge das feinabgestimmte Modell als alternatives Coach-Modell in deine RAG-Pipeline aus der Meta-Lektion hinzu.
- Demonstriere, dass es fünf Fragen zu deinem gewählten Thema korrekt beantwortet.

**Dokumentation**
- Schreibe eine kurze `README.md`, die beschreibt: dein Thema, Datensatz-Erstellungsprozess, Trainingskonfiguration und Ergebnisse.
- Committe alles unter `stage3-project/` in deinem `robot-sandbox`-Repo.

## Zusatzaufgaben

- Vergleiche Basis- vs feinabgestimmtes Modell auf deinem Validierungsset mit einem automatisierten Bewertungsskript.
- Veröffentliche deinen Datensatz auf dem Hugging Face Hub.
- Füge deine neue Lektions-Markdown-Datei zu `courses/` hinzu und indiziere sie, damit der RAG-Coach auch Fragen dazu beantworten kann.

## Reflexion

Präsentiere dein feinabgestimmtes Modell dem Roboter und erkläre:

1. Was die Feinabstimmung tatsächlich in den Gewichten des Modells verändert hat, auf konzeptioneller Ebene.
2. Wie du sichergestellt hast, dass dein Datensatz vielfältig genug war, um Overfitting auf einen engen Formulierungsstil zu vermeiden.
3. Einen Fehlerfall, den du beobachtet hast, und was du tun würdest, um ihn zu beheben.
