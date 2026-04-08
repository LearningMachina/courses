# The Linux Command Line

## Concept

An operating system is the software that manages hardware and lets you run programs. Linux is an open-source OS that powers most servers, embedded systems, and robots — including yours. The terminal is a text interface to the OS: instead of clicking, you type commands.

## Topics

- What is an operating system? What makes Linux different?
- The terminal: navigating the filesystem (`cd`, `ls`, `pwd`, `mkdir`, `rm`)
- Reading and writing files (`cat`, `nano`, `less`, `echo`, redirection)
- Processes: running programs, backgrounding, killing (`ps`, `top`, `kill`, `&`)
- Permissions: what `rwx` means and why it matters
- Package management: installing software with `apt`
- Shell scripting basics: variables, loops, if/else in bash
- SSH: connecting to the robot remotely from another machine

## Example

```bash
# Navigate and explore the filesystem
pwd                        # print working directory
ls -la                     # list files with permissions
cd /home/robot             # change directory
mkdir my_project           # create a folder
echo "hello robot" > hi.txt  # write text to a file
cat hi.txt                 # read it back

# Run a process in the background
python3 sensor_reader.py &
ps aux | grep python       # find it
kill 1234                  # stop it by PID

# A simple bash script
#!/bin/bash
for i in 1 2 3; do
  echo "Loop iteration $i"
done
```

## Exercise

Open a terminal on the robot and:

1. Print your current directory with `pwd`.
2. Create a folder called `practice` inside your home directory.
3. Inside it, create a file called `hello.txt` containing the text `Hello, robot!`.
4. List the file with `ls -l` and note the permissions.
5. Write a two-line bash script that prints the date and the current user (`whoami`), then run it.

## Check

Explain to the robot:

- What does `chmod 755 script.sh` do, and why would you need it before running the script?
- What is the difference between `>` and `>>` when redirecting output?
- Why is SSH useful for working with the robot?
