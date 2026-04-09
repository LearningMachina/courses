# C++-Grundlagen

## Konzept

C++ ist der Industriestandard für performancekritischen Robotik-Code: Es gibt dir direkte Kontrolle über den Speicher und läuft nah an der Hardware. ROS2, Motortreiber und Echtzeit-Regelschleifen werden fast immer in C++ geschrieben. Du musst es nicht auf einmal meistern — beginne mit den Grundlagen und baue darauf auf.

## Themen

- Warum C++? Geschwindigkeit, Speicherkontrolle, Robotik-Industriestandard
- Typen, Pointer, Referenzen
- Kontrollfluss und Funktionen
- Structs und Klassen
- Kompilierung, Linking, Build-Systeme (CMake-Grundlagen)
- Ein einfaches Motorsteuerungsprogramm in C++ schreiben

## Code

```cpp
// motor.hpp
#pragma once

class Motor {
public:
    Motor(const std::string& name, int pin);
    void setSpeed(float speed);   // -1.0 bis 1.0
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
    // TODO: PWM-Wert auf pin_ schreiben
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

## Aktion

1. Kompiliere und starte das obige Beispiel.
2. Füge eine `stop()`-Methode hinzu, die die Geschwindigkeit auf `0.0` setzt.
3. Schreibe eine freie Funktion `driveForward(Motor& left, Motor& right, float speed)` und rufe sie aus `main` auf.
4. Führe Pointer ein: Speichere zwei `Motor`-Objekte auf dem Heap mit `new`, nutze sie, und lösche sie dann mit `delete`.

## Reflexion

Nachdem du das Verhalten des Roboters beobachtet hast, reflektiere:

- Was ist der Unterschied zwischen der Übergabe einer Variablen per Wert, per Pointer und per Referenz?
- Was passiert, wenn du ein Objekt mit `new` erstellst und es nie mit `delete` löschst?
- Warum verwenden Robotik-Projekte CMake statt `g++` direkt aufzurufen?

## Erweiterung

Verändere den C++-Code, um das Motorverhalten des Roboters zu ändern:

1. Ändere den Duty-Cycle in `setSpeed()` auf eine nichtlineare Kurve (z. B. quadriere den Eingang) — das Beschleunigungsprofil des Roboters ändert sich von linear zu sanft.
2. Füge eine `reverse()`-Methode hinzu, die die aktuelle Geschwindigkeit negiert — der Roboter kann jetzt rückwärts fahren.
3. Erstelle zwei `Motor`-Objekte und schreibe eine Funktion, die sie in entgegengesetzte Richtungen drehen lässt — der Roboter dreht sich jetzt auf der Stelle. Beobachte den Zusammenhang zwischen Code-Struktur und physischer Bewegung.
