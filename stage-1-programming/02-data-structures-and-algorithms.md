# Data Structures & Algorithms

## Concept

The Jetson Orin Nano is powerful but not infinite — choosing the right data structure can mean the difference between a robot that responds in milliseconds and one that lags. Understanding how common structures and algorithms behave lets you write code that is both correct and fast enough to run in real time.

## Topics

- Why efficiency matters on constrained hardware
- Lists vs dictionaries: when to use which
- Stacks, queues, and why robots use them
- Sorting and searching: intuition behind common algorithms
- Big-O notation: a simple mental model (no maths required)
- Recursion: what it is, when it helps, when it hurts

## Example

```python
from collections import deque

# Stack (LIFO) — e.g. undo history, depth-first search
stack = []
stack.append("move_forward")
stack.append("turn_left")
last = stack.pop()          # "turn_left"

# Queue (FIFO) — e.g. sensor event buffer, command queue
queue = deque()
queue.append("read_camera")
queue.append("read_lidar")
first = queue.popleft()     # "read_camera"

# Dict lookup is O(1); list search is O(n)
sensor_map = {"lidar": 0, "camera": 1, "imu": 2}
idx = sensor_map["imu"]     # instant, regardless of size

# Binary search on a sorted list is O(log n)
import bisect
distances = [1.2, 2.5, 3.8, 5.0, 7.1]
pos = bisect.bisect_left(distances, 3.5)   # insert position

# Recursion example: flatten nested sensor config
def flatten(d, prefix=""):
    result = {}
    for k, v in d.items():
        key = f"{prefix}.{k}" if prefix else k
        if isinstance(v, dict):
            result.update(flatten(v, key))
        else:
            result[key] = v
    return result
```

## Exercise

1. Implement a simple task queue for the robot: commands are added with `enqueue(cmd)` and consumed with `dequeue()`. Use `deque` internally.
2. Given a list of 10 000 random integers, measure (with `time.perf_counter`) how long it takes to check membership with a list vs a set.
3. Write a recursive function `sum_nested(lst)` that sums all numbers in an arbitrarily nested list like `[1, [2, [3, 4]], 5]`.

## Check

Explain to the robot:

- What does O(n²) mean in plain English, and which common sorting algorithm has that complexity in the worst case?
- When would you choose a queue over a stack for a robot behaviour?
- Why can recursion be dangerous on a microcontroller or deeply embedded system?
