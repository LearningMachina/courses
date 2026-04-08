# Stage 0 Project — System Status Script

## Goal

Write a bash script that prints the robot's CPU temperature, battery level, and a greeting message every 5 seconds.

## Requirements

- The script runs indefinitely until interrupted with `Ctrl+C`.
- Every 5 seconds it prints:
  - Current date and time
  - CPU temperature (read from `/sys/class/thermal/thermal_zone0/temp` or `tegrastats`)
  - Battery percentage (read from the appropriate `/sys/class/power_supply/` path)
  - A short greeting that includes the robot's hostname
- The script is executable (`chmod +x`) and has a proper shebang line (`#!/bin/bash`).
- It is committed to your `robot-sandbox` Git repository.

## Starter Template

```bash
#!/bin/bash

get_cpu_temp() {
  raw=$(cat /sys/class/thermal/thermal_zone0/temp)
  echo "scale=1; $raw / 1000" | bc
}

get_battery() {
  cat /sys/class/power_supply/BAT0/capacity 2>/dev/null || echo "N/A"
}

while true; do
  echo "──────────────────────────────"
  echo "$(date '+%Y-%m-%d %H:%M:%S')"
  echo "CPU temp : $(get_cpu_temp) °C"
  echo "Battery  : $(get_battery) %"
  echo "Hello from $(hostname)!"
  sleep 5
done
```

## Stretch Goals

- Colour-code the temperature (green < 60 °C, yellow < 80 °C, red ≥ 80 °C) using ANSI escape codes.
- Log each status line to a file (`status.log`) as well as printing to screen.
- Accept a command-line argument to change the interval (e.g., `./status.sh 10` for 10-second updates).

## Check

Present your running script to the robot and explain:

1. What `#!/bin/bash` does and why it must be the first line.
2. How the `while true` loop works and how `sleep 5` controls timing.
3. What would happen if you removed the `2>/dev/null` from the battery command on a machine with no battery.
