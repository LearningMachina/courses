# Tools You Will Use Every Day

## Concept

Professional robotics developers rely on a small set of tools for almost everything: a text editor for writing code, Git for tracking changes, and Python virtual environments for keeping dependencies tidy. Mastering these tools early means you spend your time solving problems, not fighting your own setup.

## Topics

- Text editors: `nano` for quick edits, VS Code for real projects
- Git basics: `init`, `add`, `commit`, `push`, `pull` — why version control matters
- Python environment: what a virtual environment is and why to use one
- Running your first Python script on the robot

## Code

```bash
# --- Git ---
git init my_project        # start a new repo
cd my_project
echo "# My Robot Project" > README.md
git add README.md
git commit -m "Initial commit"
git remote add origin git@github.com:you/my_project.git
git push -u origin main

# --- Python virtual environment ---
python3 -m venv .venv          # create the environment
source .venv/bin/activate       # activate it
pip install flask               # install a package inside the env
python3 hello.py                # run a script
deactivate                      # leave the environment
```

```python
# hello.py — your first script on the robot
import platform

print(f"Hello from {platform.node()}!")
print(f"Python {platform.python_version()} on {platform.system()}")
```

## Action

1. Create a new Git repo called `robot-sandbox` in your home directory.
2. Inside it, create a Python virtual environment called `.venv` and activate it.
3. Install the `requests` library (`pip install requests`).
4. Write a script `hello.py` that prints the robot's hostname and Python version.
5. Add and commit both `hello.py` and a `.gitignore` that excludes `.venv/`.

## Reflection

After observing the robot's behavior, reflect on:

- What problem does a virtual environment solve? What goes wrong without one?
- What is the difference between `git add` and `git commit`?
- If you accidentally delete `hello.py`, how do you recover it using Git?

## Extension

Modify your workflow to change the robot's development behavior:

1. Edit your `hello.py` to print a different message, commit the change, and then use `git log` to see both versions — the robot's history is now visible.
2. Create a branch called `experiment`, make a change there, then switch back to `main` — observe how the robot's code can exist in two states at once.
3. Add a second Python file that imports from the first — the robot's code now has structure and dependencies.
