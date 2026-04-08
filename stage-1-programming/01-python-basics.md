# Python Basics

## Concept

Python is the primary language for robotics prototyping: it is readable, has vast libraries, and runs well on the Jetson. These fundamentals — variables, control flow, functions, and data structures — are the building blocks for every program you will write on the robot.

## Topics

- Variables, data types, operators
- Control flow: `if`/`else`, `for` loops, `while` loops
- Functions: defining, calling, parameters, return values
- Lists, dictionaries, tuples
- File I/O: reading and writing text files
- Modules and imports: using the standard library
- Introduction to classes and objects
- Error handling: `try`/`except`, writing defensive code

## Example

```python
# Variables and types
speed = 0.75          # float
name = "LearningMachina"
is_moving = False

# Control flow
if speed > 0:
    print("Robot is moving")
elif speed == 0:
    print("Robot is stopped")
else:
    print("Reversing")

# Function
def clamp(value, low, high):
    """Clamp value to [low, high] range."""
    return max(low, min(value, high))

print(clamp(1.5, 0.0, 1.0))   # 1.0

# List and dict
sensors = [12.3, 11.9, 13.1]
state = {"battery": 87, "temperature": 42.5}

# File I/O
with open("log.txt", "w") as f:
    f.write(f"battery={state['battery']}\n")

# Class
class Motor:
    def __init__(self, name):
        self.name = name
        self.speed = 0.0

    def set_speed(self, speed):
        self.speed = clamp(speed, -1.0, 1.0)

left = Motor("left")
left.set_speed(0.8)
print(f"{left.name}: {left.speed}")

# Error handling
try:
    val = int(input("Enter a number: "))
except ValueError as e:
    print(f"That wasn't a number: {e}")
```

## Exercise

1. Write a function `celsius_to_fahrenheit(c)` and test it with several values.
2. Create a `Robot` class with `name`, `battery` (int, 0–100), and a method `status()` that prints a summary line.
3. Write a loop that reads lines from a file and prints only lines that start with `#`.
4. Wrap the file reading in a `try`/`except` so that a missing file prints a friendly error instead of a traceback.

## Check

Explain to the robot:

- What is the difference between a list and a dictionary? Give a robotics example for each.
- Why should you use `with open(...)` instead of `f = open(...)` followed by `f.close()`?
- What happens when an exception is raised and there is no `try`/`except` around it?
