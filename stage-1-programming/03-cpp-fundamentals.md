# C++ Fundamentals

## Concept

C++ is the industry standard for performance-critical robotics code: it gives you direct control over memory and runs close to the hardware. ROS2, motor drivers, and real-time control loops are almost always written in C++. You do not need to master it all at once — start with the essentials and build up.

## Topics

- Why C++? Speed, memory control, robotics industry standard
- Types, pointers, references
- Control flow and functions
- Structs and classes
- Compilation, linking, build systems (CMake basics)
- Writing a simple motor control program in C++

## Example

```cpp
// motor.hpp
#pragma once

class Motor {
public:
    Motor(const std::string& name, int pin);
    void setSpeed(float speed);   // -1.0 to 1.0
    float getSpeed() const;

private:
    std::string name_;
    int pin_;
    float speed_ = 0.0f;
};
```

```cpp
// motor.cpp
#include "motor.hpp"
#include <algorithm>
#include <iostream>

Motor::Motor(const std::string& name, int pin)
    : name_(name), pin_(pin) {}

void Motor::setSpeed(float speed) {
    speed_ = std::clamp(speed, -1.0f, 1.0f);
    std::cout << name_ << " speed -> " << speed_ << "\n";
    // TODO: write PWM value to pin_
}

float Motor::getSpeed() const { return speed_; }
```

```cpp
// main.cpp
#include "motor.hpp"

int main() {
    Motor left("left", 12);
    Motor right("right", 13);
    left.setSpeed(0.8f);
    right.setSpeed(0.8f);
    return 0;
}
```

```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 3.16)
project(motor_demo)
set(CMAKE_CXX_STANDARD 17)
add_executable(motor_demo main.cpp motor.cpp)
```

```bash
mkdir build && cd build
cmake .. && make
./motor_demo
```

## Exercise

1. Compile and run the example above.
2. Add a `stop()` method that sets speed to `0.0`.
3. Write a free function `driveForward(Motor& left, Motor& right, float speed)` and call it from `main`.
4. Introduce a pointer: store two `Motor` objects on the heap with `new`, use them, then `delete` them.

## Check

Explain to the robot:

- What is the difference between passing a variable by value, by pointer, and by reference?
- What happens if you `new` an object and never `delete` it?
- Why do robotics projects use CMake instead of calling `g++` directly?
