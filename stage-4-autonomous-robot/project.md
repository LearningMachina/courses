# Stage 4 Graduation Project — Original Autonomous Behaviour

## Goal

Design, build, and document an original autonomous behaviour. Present it to the robot — it will ask you to explain every layer of the stack.

## Requirements

**Behaviour**
- Must be original: not a direct copy of any behaviour built in Stages 0–3.
- Must integrate at least three of the five capability layers:
  1. Motor control
  2. Sensor perception (distance, vision, audio, or IMU)
  3. AI reasoning (LLM, object detection, or trained classifier)
  4. Voice interaction (STT and/or TTS)
  5. Web dashboard or API

**Code quality**
- Implemented as a ROS2 package (or equivalent modular architecture).
- At least 10 passing unit tests covering core logic.
- No hardcoded magic numbers: all tuneable parameters are named constants or config values.

**Documentation**
- `README.md` with: goal, demo video link, architecture diagram, setup instructions, and known limitations.
- Architecture diagram showing every node/component and the data flowing between them.
- Short write-up (500 words) explaining design decisions and what you would do differently next time.

**Repository**
- All code, documentation, and the dataset (if you trained a model) committed to your `robot-sandbox` repo under `graduation-project/`.

## Presentation to the Robot

The robot will ask you questions across all five stages. Be ready to explain:

- **Stage 0** — How you SSH'd into the robot during development and what shell tools helped you debug.
- **Stage 1** — Which data structures you used and why; how you handled errors gracefully.
- **Stage 2** — How sensors are read, filtered, and fed into the control loop.
- **Stage 3** — What AI model(s) you used, how you evaluated them, and what their failure modes are.
- **Stage 4** — How all layers connect end-to-end; what the latency is; how you would make it better.

## Evaluation Criteria

| Criterion                        | Weight |
|----------------------------------|--------|
| Behaviour works reliably (demo)  | 30 %   |
| Code quality and tests           | 20 %   |
| Documentation clarity            | 20 %   |
| Depth of explanation to the robot| 20 %   |
| Creativity and ambition          | 10 %   |

## Ideas (pick one or invent your own)

- A robot that plays a simple quiz game: asks questions aloud, listens for answers, keeps score.
- A security patrol bot that maps a room, detects intruders, and sends an alert to the dashboard.
- A robot that sorts coloured blocks using vision and a gripper attachment.
- A robot tutor that teaches another student the Stage 0 Linux commands interactively.
- A robot that learns a route by watching a human drive it once, then repeats it autonomously.

---

*Completing this project means you have built a robot that thinks, sees, speaks, and acts — entirely offline, on hardware that fits in your hand. That is not a small thing.*
