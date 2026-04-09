# Agentic AI

## Concept

An AI agent is a system that perceives its environment, reasons about goals, and takes actions — possibly calling tools, running code, or controlling hardware. Giving an LLM the ability to call functions turns it from a text generator into an actor that can change the world, including driving your robot.

## Topics

- What is an AI agent? Goals, tools, and feedback loops
- Function calling and tool use: giving a model the ability to act
- Building a simple robot agent: the LLM decides what motor command to send
- Memory and context: how agents remember across turns
- Multi-step reasoning: chain-of-thought and ReAct patterns
- Safety and guardrails: when not to let the model decide

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

1. Run the agent example with a local Ollama model and observe its tool calls.
2. Add a second tool `speak(text)` that makes the robot say something, and ask the agent to narrate its actions.
3. Implement a three-turn ReAct loop: sense → agent decides → act → sense again → agent decides → act.
4. Add a safety guardrail: if the agent tries to set `speed > 0.8`, cap it at `0.8` and log a warning.

## Reflection

After observing the robot's behavior, reflect on:

- What is the difference between a single-turn LLM call and an agentic loop?
- Why are guardrails important when an LLM is controlling physical hardware?
- What is chain-of-thought prompting, and how does it improve reasoning on complex tasks?

## Extension

Modify the agent to change the robot's autonomous decision-making:

1. Add a `look_around()` tool that rotates the robot 360° and reports what YOLO detects — the robot can now gather information before deciding what to do.
2. Change the agent from single-turn to a 5-turn ReAct loop — observe how the robot's behavior becomes more deliberate: it thinks, acts, observes, and adjusts.
3. Add a guardrail that prevents the robot from driving if battery is below 20% — the robot now has self-preservation. Remove the guardrail and observe: the robot will drive until it dies. Safety must be explicit.
