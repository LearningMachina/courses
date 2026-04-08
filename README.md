# LearningMachina — Course Curriculum

A structured path from complete beginner to autonomous robot builder.
All material is designed to be taught interactively by the robot itself —
ask it anything, at any point, and it will explain, demonstrate, and guide you.

---

## Lesson Format

Every lesson follows the same structure so the robot knows how to teach it:

1. **Concept** — the robot explains the idea in 2–3 sentences
2. **Example** — a short code snippet or diagram
3. **Exercise** — the student tries it on the robot itself
4. **Check** — the student explains it back; the robot gives feedback

All lessons are plain Markdown files. The robot's LLM reads them as context,
so the student can ask any follow-up question and get a grounded answer.

---

## Stage 0 — Linux & the Machine

*Get comfortable with the environment. Everything else builds on this.*

**The Linux command line**
- What is an operating system? What makes Linux different?
- The terminal: navigating the filesystem (cd, ls, pwd, mkdir, rm)
- Reading and writing files (cat, nano, less, echo, redirection)
- Processes: running programs, backgrounding, killing (ps, top, kill, &)
- Permissions: what rwx means and why it matters
- Package management: installing software with apt
- Shell scripting basics: variables, loops, if/else in bash
- SSH: connecting to the robot remotely from another machine

**The robot's hardware**
- What is a Jetson Orin Nano? CPU vs GPU explained simply
- The LEGO-compatible case: how it works and what can be attached
- Ports and connectors: USB, GPIO, UART, I2C, SPI, camera
- The motor driver board: what it does and how to wire it
- Powering the robot: battery, voltage rails, always power off before wiring
- Your first interaction: turning an LED on from the command line

**Tools you will use every day**
- Text editors: nano for quick edits, VS Code for real projects
- Git basics: init, add, commit, push, pull — why version control matters
- Python environment: what a virtual environment is and why to use one
- Running your first Python script on the robot

> **Stage 0 project:** Write a bash script that prints the robot's CPU temperature,
> battery level, and a greeting message every 5 seconds.

---

## Stage 1 — Programming

*Learn to think like a computer. Write real code that runs on the robot.*

**Python basics** (start here)
- Variables, data types, operators
- Control flow: if/else, for loops, while loops
- Functions: defining, calling, parameters, return values
- Lists, dictionaries, tuples
- File I/O: reading and writing text files
- Modules and imports: using the standard library
- Introduction to classes and objects
- Error handling: try/except, writing defensive code

**Data structures & algorithms**
- Why efficiency matters on constrained hardware
- Lists vs dictionaries: when to use which
- Stacks, queues, and why robots use them
- Sorting and searching: intuition behind common algorithms
- Big-O notation: a simple mental model (no maths required)
- Recursion: what it is, when it helps, when it hurts

**C++ fundamentals**
- Why C++? Speed, memory control, robotics industry standard
- Types, pointers, references
- Control flow and functions
- Structs and classes
- Compilation, linking, build systems (CMake basics)
- Writing a simple motor control program in C++

**Rust introduction**
- Why Rust? Safety without garbage collection, embedded systems
- Ownership, borrowing, lifetimes
- Error handling with Result and Option
- Traits and generics
- Async/await basics
- How the robot's own control software is written in Rust

**Networking & APIs**
- How computers talk: IP addresses, ports, HTTP basics
- Writing a simple HTTP server in Python (Flask / FastAPI)
- REST APIs: sending and receiving JSON
- WebSockets: real-time communication between robot and browser
- Building a minimal web dashboard for your robot's sensor data
- Node.js in one lesson: when to use JavaScript on the backend

> **Stage 1 project:** Build a Python web server that exposes the robot's
> sensor readings as a live JSON API, browsable from any device on the local network.

---

## Stage 2 — Robotics

*Make the robot move, sense, and react to the physical world.*

**Electronics basics**
- Voltage, current, resistance — just enough to not break things
- Reading a datasheet: what the important numbers actually mean
- GPIO pins: digital output (on/off), digital input, pull-up/pull-down
- Breadboarding: prototyping circuits safely before soldering
- Common components: LEDs, buttons, resistors, capacitors
- Power safety: why you never connect 5 V where 3.3 V is expected

**Motor control**
- How DC motors and motor drivers work
- PWM: pulse-width modulation explained
- Differential drive: how tracked robots steer
- PID control: keeping movement smooth and accurate
- Writing a simple drive loop in Python

**Sensors**
- Types of sensors: distance (ultrasonic, IR, lidar), IMU, encoders
- Reading sensor data over I2C, UART, SPI
- Filtering noisy sensor data (moving average, Kalman basics)
- Reacting to the environment: obstacle avoidance

**Computer vision**
- How cameras work: resolution, frame rate, exposure
- OpenCV basics: capturing frames, colour spaces, thresholding
- Edge detection, contour finding
- Object detection: what YOLO does and how it works
- Depth perception: stereo cameras and disparity maps
- Running vision on the robot: CUDA acceleration on Jetson

**System integration**
- ROS2 basics: nodes, topics, messages
- Writing a complete sense-plan-act loop
- Logging, debugging, and testing on hardware

> **Stage 2 project:** Build an obstacle-avoiding robot that also streams
> its camera feed to a browser dashboard you wrote in Stage 1.

---

## Stage 3 — AI & Machine Learning

*Teach the robot to learn from data and understand the world.*

**Machine learning foundations**
- What is machine learning? Supervised vs unsupervised vs reinforcement
- Linear regression and logistic regression from scratch
- Train/validation/test splits, overfitting, regularisation
- Gradient descent: the engine of learning
- Introduction to PyTorch

**Deep learning**
- Neural networks: layers, activations, backpropagation
- Training a network on the robot's own sensor data
- Convolutional Neural Networks (CNNs)
- How the robot's YOLO detector was trained
- Transfer learning: fine-tuning a pretrained model

**Transformers and language models**
- Attention mechanism: intuition and mathematics
- The transformer architecture
- Large Language Models: GPT, Llama, Gemma, Mistral
- Running LLMs locally with Ollama on Jetson Orin Nano
- Prompt engineering and system prompts

**Agentic AI**
- What is an AI agent? Goals, tools, and feedback loops
- Function calling and tool use: giving a model the ability to act
- Building a simple robot agent: the LLM decides what motor command to send
- Memory and context: how agents remember across turns
- Multi-step reasoning: chain-of-thought and ReAct patterns
- Safety and guardrails: when not to let the model decide

**Multimodal models**
- Vision-Language Models (VLMs): seeing and understanding at once
- Vision-Language-Action models (VLAs): from perception to robot action
- Speech-to-Text (STT): Whisper, Voxtral
- Text-to-Speech (TTS): Piper, Kokoro, expressive voices
- End-to-end voice interaction pipelines

**How the AI coach works** *(meta-lesson)*
- Fine-tuning vs RAG: how the coach was built on this curriculum
- The voice pipeline: STT → LLM → TTS end to end
- System prompts and persona: why the coach answers the way it does
- Evaluating a model: how do you know if it's teaching well?
- How to contribute a new lesson so the coach learns it too

> **Stage 3 project:** Fine-tune a small local model on a topic of your
> choice and add it to the coach's knowledge base.

---

## Stage 4 — Autonomous Robot

*Bring everything together. Build a robot that thinks, sees, speaks, and acts.*

**Full system architecture**
- How all layers connect: sensors → perception → reasoning → action
- Latency and real-time constraints
- Running everything offline: no cloud, no subscription
- Profiling and optimisation: finding bottlenecks on Jetson hardware

**Autonomous behaviours**
- Person detection and greeting
- Voice-activated Q&A with the onboard LLM
- Navigating an obstacle course using vision and motor control
- Following a person using optical flow
- Responding to voice commands with physical actions

**Building your own projects**
- Designing a robot behaviour from scratch
- Debugging hardware + software systems together
- Documenting and sharing your project
- Writing tests for robot code: why hardware-in-the-loop testing matters

**Contributing back**
- Adding a new lesson to the curriculum
- Training a custom model on your own data
- Submitting a project to the LearningMachina community
- Opening a pull request: how open-source collaboration works

> **Stage 4 graduation project:** Design, build, and document an original
> autonomous behaviour. Present it to the robot — it will ask you to explain
> every layer of the stack.
