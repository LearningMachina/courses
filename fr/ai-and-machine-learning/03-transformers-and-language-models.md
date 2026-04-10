# Transformers et modèles de langage

## Concept

L'architecture transformer — construite sur le mécanisme d'attention — a révolutionné l'IA en 2017. Aujourd'hui, elle alimente tous les grands modèles de langage : GPT, Llama, Gemma, Mistral. Sur le Jetson Orin Nano, vous pouvez exécuter localement des modèles de 3 à 7 milliards de paramètres, offrant à votre robot un cerveau capable de raisonner, d'expliquer et de converser sans aucune connexion au cloud.

## Thèmes

- Mécanisme d'attention : intuition et mathématiques
- L'architecture transformer
- Grands modèles de langage : GPT, Llama, Gemma, Mistral
- Exécuter des LLM localement avec Ollama sur Jetson Orin Nano
- Ingénierie de prompts et prompts système

## Code

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

## Action

1. Installez Ollama, téléchargez `gemma:2b` ou `llama3.2:1b`, et lancez une conversation depuis la ligne de commande.
2. Écrivez un script Python qui envoie les lectures actuelles des capteurs du robot (formatées sous forme de chaîne de caractères) au modèle et lui demande de décider : avancer, tourner à gauche, tourner à droite, ou s'arrêter.
3. Expérimentez avec trois prompts système différents et notez comment ils modifient le ton et le style du modèle.
4. Mesurez le débit en tokens par seconde sur le Jetson avec et sans déchargement GPU.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux points suivants :

- Que signifie « fenêtre de contexte », et que se passe-t-il lorsqu'on la dépasse ?
- Quelle est la différence entre un modèle de base et un modèle affiné pour suivre des instructions ?
- Pourquoi la quantification (par ex., Q4_K_M) permet-elle de faire tenir un modèle plus grand sur le Jetson, et quel est le compromis ?

## Extension

Modifiez la configuration du LLM pour changer la façon dont le robot pense et s'exprime :

1. Changez le prompt système d'un assistant serviable à un responsable de sécurité prudent — posez la même question et observez comment la personnalité et les décisions du robot changent complètement.
2. Passez de `gemma:2b` à un modèle plus grand (si la mémoire le permet) ou plus petit — mesurez le compromis entre vitesse et qualité. Le robot devient soit plus intelligent mais plus lent, soit plus rapide mais plus simple.
3. Envoyez des données de capteurs en temps réel dans le prompt et demandez au robot de narrer ce qu'il vit — le robot décrit désormais son propre état en langage naturel. Il a une voix.
