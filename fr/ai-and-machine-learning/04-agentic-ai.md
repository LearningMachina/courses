# IA agentique

## Concept

Un agent IA est un système qui perçoit son environnement, raisonne sur des objectifs et entreprend des actions — pouvant appeler des outils, exécuter du code ou contrôler du matériel. Donner à un LLM la capacité d'appeler des fonctions le transforme d'un simple générateur de texte en un acteur capable de modifier le monde, y compris de piloter votre robot.

## Thèmes

- Qu'est-ce qu'un agent IA ? Objectifs, outils et boucles de rétroaction
- Appels de fonctions et utilisation d'outils : donner à un modèle la capacité d'agir
- Construire un agent robotique simple : le LLM décide quelle commande moteur envoyer
- Mémoire et contexte : comment les agents se souviennent d'un tour à l'autre
- Raisonnement en plusieurs étapes : chaîne de pensée et patterns ReAct
- Sécurité et garde-fous : quand ne pas laisser le modèle décider

## Code

```python
import ollama, json

# Define the tools the agent may call
tools = [
    {
        "type": "function",
        "function": {
            "name": "drive",
            "description": "Set robot driving direction and speed.",
            "parameters": {
                "type": "object",
                "properties": {
                    "direction": {"type": "string", "enum": ["forward", "backward", "left", "right", "stop"]},
                    "speed":     {"type": "number", "description": "0.0 to 1.0"},
                },
                "required": ["direction", "speed"],
            },
        },
    }
]

def drive(direction: str, speed: float):
    print(f"[MOTOR] {direction} @ {speed:.2f}")
    # TODO: translate to actual PWM commands

def agent_turn(sensor_summary: str):
    messages = [
        {"role": "system", "content": "You control a tracked robot. Use the drive tool to respond to sensor data."},
        {"role": "user",   "content": sensor_summary},
    ]
    response = ollama.chat(model="llama3.2:1b", messages=messages, tools=tools)

    for tool_call in response.get("message", {}).get("tool_calls", []):
        name = tool_call["function"]["name"]
        args = tool_call["function"]["arguments"]
        if name == "drive":
            drive(**args)

# Run one agent turn
agent_turn("Front distance: 0.25 m. Left distance: 0.8 m. Right distance: 1.2 m.")
```

```
ReAct pattern: Reason → Act → Observe → Reason → …
  Thought: The obstacle is close on the front and left. I should turn right.
  Action:  drive(direction="right", speed=0.5)
  Observation: Front distance is now 0.9 m.
  Thought: Path is clear. Drive forward.
  Action:  drive(direction="forward", speed=0.6)
```

## Action

1. Exécutez l'exemple d'agent avec un modèle Ollama local et observez ses appels d'outils.
2. Ajoutez un deuxième outil `speak(text)` qui fait parler le robot, et demandez à l'agent de narrer ses actions.
3. Implémentez une boucle ReAct en trois tours : percevoir → l'agent décide → agir → percevoir à nouveau → l'agent décide → agir.
4. Ajoutez un garde-fou de sécurité : si l'agent tente de définir `speed > 0.8`, limitez à `0.8` et enregistrez un avertissement.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux points suivants :

- Quelle est la différence entre un appel LLM unique et une boucle agentique ?
- Pourquoi les garde-fous sont-ils importants lorsqu'un LLM contrôle du matériel physique ?
- Qu'est-ce que le prompting par chaîne de pensée, et comment améliore-t-il le raisonnement sur des tâches complexes ?

## Extension

Modifiez l'agent pour changer la prise de décision autonome du robot :

1. Ajoutez un outil `look_around()` qui fait tourner le robot à 360° et rapporte ce que YOLO détecte — le robot peut désormais collecter des informations avant de décider quoi faire.
2. Faites passer l'agent d'un seul tour à une boucle ReAct de 5 tours — observez comment le comportement du robot devient plus réfléchi : il pense, agit, observe et ajuste.
3. Ajoutez un garde-fou empêchant le robot de rouler si la batterie est en dessous de 20 % — le robot a désormais un instinct de survie. Retirez le garde-fou et observez : le robot roulera jusqu'à l'épuisement. La sécurité doit être explicite.
