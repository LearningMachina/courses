# Transformers and Language Models

## Concept

The transformer architecture — built on the attention mechanism — changed AI in 2017. Today it powers every major language model: GPT, Llama, Gemma, Mistral. On the Jetson Orin Nano you can run 3–7 B parameter models locally, giving your robot a brain that can reason, explain, and converse without any cloud connection.

## Topics

- Attention mechanism: intuition and mathematics
- The transformer architecture
- Large Language Models: GPT, Llama, Gemma, Mistral
- Running LLMs locally with Ollama on Jetson Orin Nano
- Prompt engineering and system prompts

## Example

```bash
# Install Ollama and pull a model small enough for the Jetson
curl -fsSL https://ollama.com/install.sh | sh
ollama pull gemma:2b          # ~1.5 GB, runs well on Jetson Orin Nano
ollama run gemma:2b           # interactive chat

# Or call it from Python
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
                "You are the onboard AI of a small tracked robot called LearningMachina. "
                "Answer concisely. You can control the robot by outputting JSON commands."
            ),
        },
        {"role": "user", "content": "What can you see in front of you?"},
    ],
)
print(response["message"]["content"])
```

```
Attention — intuition
  For each token, the model asks:
    "Which other tokens in this sequence are most relevant to me?"
  It assigns weights (attention scores) and takes a weighted sum.
  Multi-head attention does this several times in parallel,
  each head learning to attend to different kinds of relationships.
```

## Exercise

1. Install Ollama, pull `gemma:2b` or `llama3.2:1b`, and have a conversation from the command line.
2. Write a Python script that sends the robot's current sensor readings (formatted as a string) to the model and asks it to decide: forward, turn-left, turn-right, or stop.
3. Experiment with three different system prompts and note how they change the model's tone and style.
4. Measure the tokens-per-second throughput on the Jetson with and without GPU offloading.

## Check

Explain to the robot:

- What does "context window" mean, and what happens when you exceed it?
- What is the difference between a base model and an instruction-tuned model?
- Why does quantisation (e.g., Q4_K_M) allow a larger model to fit on the Jetson, and what is the trade-off?
