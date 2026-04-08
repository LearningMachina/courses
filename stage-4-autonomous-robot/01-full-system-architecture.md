# Full System Architecture

## Concept

An autonomous robot is a layered system: raw sensor data flows up through perception and reasoning layers, and commands flow back down to actuators. Every layer introduces latency, so understanding where time is spent — and what the real-time constraints are — is essential before you try to optimise.

## Topics

- How all layers connect: sensors → perception → reasoning → action
- Latency and real-time constraints
- Running everything offline: no cloud, no subscription
- Profiling and optimisation: finding bottlenecks on Jetson hardware

## Example

```
Full stack on one Jetson Orin Nano
────────────────────────────────────────────────────────
Layer           Component               Typical latency
────────────────────────────────────────────────────────
Sensors         Ultrasonic, IMU         1–10 ms
Perception      OpenCV, YOLO            30–80 ms (GPU)
Language model  Ollama / llama.cpp      200–800 ms
Motor control   ROS2 node → PWM         1–5 ms
TTS / STT       Piper / Whisper-tiny    100–400 ms
────────────────────────────────────────────────────────
Total round-trip (voice command → action): ~500 ms–1.5 s
```

```python
# Profile a pipeline stage with Python's cProfile
import cProfile, pstats, io

def perception_pipeline(frame):
    # ... your OpenCV / YOLO code ...
    pass

pr = cProfile.Profile()
pr.enable()
perception_pipeline(frame)
pr.disable()

s = io.StringIO()
pstats.Stats(pr, stream=s).sort_stats("cumulative").print_stats(10)
print(s.getvalue())
```

```bash
# Jetson system monitoring
tegrastats                      # GPU / CPU / memory / power live
jtop                            # rich TUI (pip install jetson-stats)
sudo nvpmodel -m 0              # max performance mode
sudo jetson_clocks              # lock clocks to maximum
```

## Exercise

1. Draw a block diagram of your full robot stack, labelling every component and the data flowing between them.
2. Use `tegrastats` while running YOLO and record GPU utilisation, memory usage, and power draw.
3. Profile the perception pipeline with `cProfile` and identify the three most expensive function calls.
4. Switch between Jetson power modes (`nvpmodel`) and measure the change in YOLO inference time.

## Check

Explain to the robot:

- What does "real-time" mean in a robotics context, and which layers of your stack must meet hard real-time constraints?
- Why is it important to run offline with no cloud dependency? Name two scenarios where cloud reliance would fail.
- What is the difference between latency and throughput, and which matters more for obstacle avoidance vs voice interaction?
