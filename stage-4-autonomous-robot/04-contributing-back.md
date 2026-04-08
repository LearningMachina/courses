# Contributing Back

## Concept

Open-source projects grow because contributors share improvements. The LearningMachina curriculum is designed to be extended: any student who masters a topic can add a lesson, train a model, or share a project. Learning to contribute is itself a valuable professional skill — it is how most robotics software is built.

## Topics

- Adding a new lesson to the curriculum
- Training a custom model on your own data
- Submitting a project to the LearningMachina community
- Opening a pull request: how open-source collaboration works

## Example

```bash
# Fork → branch → commit → PR workflow
git clone git@github.com:YOUR_USERNAME/LearningMachina.courses.git
cd courses
git checkout -b lesson/add-kalman-filter

# Create your lesson file
cp template.md stage-2-robotics/06-kalman-filter.md
# ... edit it ...

git add stage-2-robotics/06-kalman-filter.md
git commit -m "Add Kalman filter lesson to Stage 2 Sensors"
git push origin lesson/add-kalman-filter

# Open a pull request on GitHub
gh pr create --title "Add Kalman filter lesson" \
             --body "Covers 1D Kalman filter with a worked sensor example."
```

```markdown
<!-- Lesson template (every new lesson must follow this) -->
# Lesson Title

## Concept
Two to three sentences explaining the idea.

## Topics
- Bullet list of sub-topics covered

## Example
\`\`\`python
# Runnable code
\`\`\`

## Exercise
Numbered steps the student does on the robot.

## Check
Questions the student answers back to the robot.
```

## Exercise

1. Fork the `courses` repository and clone your fork.
2. Write a new lesson on a topic not yet covered (e.g., SLAM, behaviour trees, ROS2 actions).
3. Follow the lesson template exactly.
4. Open a pull request with a clear title and description.
5. Respond to any review comments from the maintainers.

## Check

Explain to the robot:

- What is the difference between a fork and a branch?
- Why do open-source projects ask contributors to open a pull request rather than push directly to `main`?
- What makes a good commit message, and why does it matter in a collaborative project?
