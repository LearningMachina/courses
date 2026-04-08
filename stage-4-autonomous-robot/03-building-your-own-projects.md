# Building Your Own Projects

## Concept

Designing your own robot behaviour from scratch is the real test of everything you have learned. The process is the same whether the project is simple or complex: define the goal clearly, break it into layers, build the simplest version that works, then iterate. Documenting as you go is not optional — future you will thank present you.

## Topics

- Designing a robot behaviour from scratch
- Debugging hardware + software systems together
- Documenting and sharing your project
- Writing tests for robot code: why hardware-in-the-loop testing matters

## Example

```
Design process for a new behaviour
─────────────────────────────────────────────
1. Define success criteria
   "The robot stops within 20 cm of any obstacle,
    with < 2 false positives per minute."

2. Identify inputs and outputs
   Inputs : ultrasonic distance, camera frame
   Outputs: motor commands, optional spoken alert

3. Sketch the control flow (state machine or behaviour tree)
   IDLE → DRIVING → OBSTACLE_DETECTED → TURNING → DRIVING

4. Build a stub: fake all sensors, test the logic first
5. Swap in real sensors one at a time
6. Run, log, fix, repeat
```

```python
# State machine skeleton
from enum import Enum, auto

class State(Enum):
    IDLE     = auto()
    DRIVING  = auto()
    AVOIDING = auto()

state = State.IDLE

def tick(distance_m: float, voice_cmd: str | None):
    global state
    if state == State.IDLE:
        if voice_cmd == "go":
            state = State.DRIVING

    elif state == State.DRIVING:
        if distance_m < 0.3:
            state = State.AVOIDING
        # drive forward

    elif state == State.AVOIDING:
        if distance_m > 0.5:
            state = State.DRIVING
        # turn

    return state
```

```python
# Unit test for the state machine (no hardware needed)
def test_obstacle_triggers_avoidance():
    import importlib, types
    # inject fake state
    assert tick(1.0, "go") == State.DRIVING
    assert tick(0.2, None) == State.AVOIDING
    assert tick(0.8, None) == State.DRIVING

test_obstacle_triggers_avoidance()
print("All tests passed")
```

## Exercise

1. Choose a behaviour not covered in the curriculum and write a one-page design doc (goal, inputs, outputs, states).
2. Implement it as a state machine with unit tests for all state transitions.
3. Record a 60-second video of the robot executing the behaviour and note every failure.
4. Fix the top two failures and re-record.

## Check

Explain to the robot:

- What is hardware-in-the-loop testing, and why can you not fully test a physical robot in software alone?
- How do state machines make robot behaviour easier to reason about and debug?
- What should a project README contain so that someone else (or you, in six months) can reproduce it?
