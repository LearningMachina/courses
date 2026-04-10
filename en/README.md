# LearningMachina — Embodied Curriculum

A structured path from complete beginner to autonomous robot builder.

Unlike traditional courses, LearningMachina does not separate theory from practice.
**Everything the student learns directly controls the robot — and creates visible, physical behavior.**

The robot is not just a tool.
It is the **medium of learning**.

All material is designed to be taught interactively by the robot itself —
ask it anything, at any point, and it will explain, demonstrate, and guide you.

---

## Core Learning Principle

> **Code is not an abstract concept — it is behavior.**

Every concept the student learns results in:
- Movement
- Reaction
- Interaction
- Decision-making

The student does not just write code.
They **teach the robot how to behave**.

---

## Lesson Format

Every lesson follows a consistent, embodied structure:

1. **Concept**
   The robot explains the idea in 2–3 simple sentences.

2. **Code**
   The student writes or modifies a small piece of code using the robot's SDK.

3. **Action (Physical Feedback)**
   The robot executes the code immediately:
   - Moves
   - Speaks
   - Lights up
   - Reacts to the environment

4. **Reflection**
   The student explains what changed in the robot's behavior.

5. **Extension**
   The student modifies the code to change the robot's behavior again.

All lessons are plain Markdown files. The robot's LLM reads them as context,
so the student can ask any follow-up question and get a grounded answer.

---

## The Robot SDK (Core Abstraction)

All programming languages use a unified, simple interface to control the robot.

Students do not start with abstract programming problems.
They start by **controlling a physical system**.

This allows:
- Immediate feedback
- Intuitive understanding
- Cross-language consistency (Python, C++, Rust)

---

## Learning Model

Instead of:
> Theory → Exercises → Tests

LearningMachina uses:
> **Behavior → Understanding → Control**

Students learn by:
- Observing what the robot does
- Changing code and seeing different outcomes
- Building increasingly complex behaviors

---

## Behavioral Tracks

In parallel to the main progression, students develop skills across three tracks:

### Movement
- Navigation
- Precision control
- Smooth motion

### Interaction
- Voice responses
- Visual detection
- Human interaction

### Intelligence
- Decision-making
- Learning from data
- Autonomous behavior

---

## Stage 0 — Environment & First Control

*Make the robot come alive.*

Students learn the basics of Linux, terminal usage, files, and processes —
but every interaction has a **physical result**:
- Running a script → robot reacts
- Loop → repeated movement or signal
- Command → visible output on the robot

> **Stage Goal:**
> Understand that commands on a computer can directly control a machine.

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

> **Stage 0 mission:** Write a bash script that makes the robot report its
> vital signs — CPU temperature, battery level, and a spoken greeting —
> every 5 seconds. Observe the robot come alive on every loop iteration.

---

## Stage 1 — Programming as Behavior

*Learn to think like a computer — by controlling a robot.*

Each concept is tied to behavior:
- Changing a variable changes movement
- Conditions create reactions
- Loops create patterns
- Functions create reusable behaviors

> **Stage Goal:**
> Understand that code defines how a system behaves over time.

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

> **Stage 1 mission:** Build a Python web server that exposes the robot's
> sensor readings as a live JSON API, browsable from any device on the local network.
> The robot's state becomes visible in real time.

---

## Stage 2 — Game Development

*Build games to master real-time systems.*

Games are the bridge between programming and robotics — both are real-time
systems with input, state, and output. Students learn:
- 2D games in Python with Pygame
- 3D rendering in C++ with OpenGL
- Multiplayer networking in Rust
- Game architecture patterns (ECS, state machines)

> **Stage Goal:**
> Understand real-time systems, rendering, and networked multiplayer — skills
> that transfer directly to robotics and AI.

**2D games with Python**
- Game loops: input → update → render
- Pygame basics: windows, events, drawing
- Sprites, collision detection, simple physics
- Sound effects, tile maps, game state management

**3D games with C++**
- The graphics pipeline: vertices → shaders → screen
- OpenGL basics: windows, buffers, VAOs
- GLSL shaders: vertex and fragment
- Transformations, cameras, textures, lighting

**Multiplayer with Rust**
- Networking: TCP vs UDP, latency, packet loss
- Async programming with Tokio
- Authoritative server architecture
- State synchronisation, client-side prediction

**Game architecture & design**
- Entity-Component-System (ECS) pattern
- Game state machines
- Event systems, asset management
- Cross-language integration

> **Stage 2 mission:** Build a complete multiplayer game — a Rust server,
> a Python/Pygame client, real-time state sync, and at least one game mechanic.

---

## Stage 3 — Robotics & the Physical World

*Make the robot sense, react, and move intelligently.*

Focus:
- Input (sensors) → Output (movement)
- Real-world interaction
- Feedback loops

Students build systems where the robot:
- Avoids obstacles
- Reacts to surroundings
- Interprets visual input

> **Stage Goal:**
> Understand how software interacts with the physical world.

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

> **Stage 3 mission:** Build an obstacle-avoiding robot that also streams
> its camera feed to a browser dashboard you wrote in Stage 1. Watch the robot
> navigate its environment in real time.

---

## Stage 4 — AI & Learning Systems

*Teach the robot to think and adapt.*

The robot evolves from:
- Reactive → Predictive → Autonomous

Students build systems where the robot:
- Makes decisions
- Understands language
- Learns from data

> **Stage Goal:**
> Understand how machines can learn and make decisions.

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

> **Stage 4 mission:** Fine-tune a small local model on a topic of your
> choice and add it to the coach's knowledge base. Teach the robot something new.

---

## Stage 5 — Autonomous Robot

*Bring everything together into a complete system.*

Students design complete behaviors:
- Navigation
- Interaction
- Task execution

The robot becomes:
- Independent
- Responsive
- Goal-driven

> **Stage Goal:**
> Build a fully autonomous system that operates in the real world.

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

> **Stage 5 graduation mission:** Design, build, and document an original
> autonomous behaviour. Present it to the robot — it will ask you to explain
> every layer of the stack.

---

## Missions Instead of Exercises

Students do not complete abstract exercises.

They complete **missions**:
- Navigate a space
- Follow a person
- Respond to voice commands
- React to environmental changes

Each mission combines:
- Programming
- Robotics
- AI

---

## Debugging Through Behavior

Errors are not hidden in logs — they are visible:

- Wrong logic → wrong movement
- Sensor error → incorrect reaction
- State → visible through lights, sound, motion

This makes debugging:
- Intuitive
- Immediate
- Physical

---

## Final Outcome

By the end of the curriculum, the student can:

- Program in multiple languages
- Control real hardware
- Build intelligent systems
- Design autonomous behaviors

Most importantly:

> They understand how to turn code into real-world action.

---

## Philosophy Summary

LearningMachina does not teach programming as an abstract skill.

It teaches:

> **How to give a machine behavior.**
