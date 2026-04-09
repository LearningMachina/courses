# Stage 3 Mission — Fine-Tune a Local Model

## Goal

Fine-tune a small local model on a topic of your choice and add it to the coach's knowledge base.

## Requirements

**Dataset**
- Choose a topic related to robotics, electronics, or programming.
- Create a dataset of at least 50 question–answer pairs in the format used for instruction fine-tuning (e.g., Alpaca or ShareGPT format).
- Split into training (80 %) and validation (20 %) sets.

**Fine-tuning**
- Use `unsloth`, `llama.cpp` fine-tune, or HuggingFace `transformers` + `trl` to fine-tune a model ≤ 3 B parameters.
- Train on the Jetson or on a laptop/cloud instance and transfer the weights.
- Save the resulting GGUF or safetensors checkpoint.

**Integration**
- Add the fine-tuned model as an alternative coach model in your RAG pipeline from the meta-lesson.
- Demonstrate it answering five questions on your chosen topic correctly.

**Documentation**
- Write a short `README.md` describing: your topic, dataset creation process, training configuration, and results.
- Commit everything under `stage3-project/` in your `robot-sandbox` repo.

## Stretch Goals

- Compare base vs fine-tuned model on your validation set using an automated scoring script.
- Publish your dataset to Hugging Face Hub.
- Add your new lesson Markdown file to `courses/` and index it so the RAG coach can also answer questions about it.

## Reflection

Present your fine-tuned model to the robot and explain:

1. What fine-tuning actually changed in the model's weights, at a conceptual level.
2. How you ensured your dataset was diverse enough to avoid overfitting to a narrow phrasing style.
3. One failure case you observed and what you would do to fix it.
