# Les fondamentaux du C++

## Concept

Le C++ est la norme industrielle pour le code robotique critique en termes de performances : il vous donne un contrôle direct sur la mémoire et s'exécute au plus près du matériel. ROS2, les pilotes de moteurs et les boucles de contrôle en temps réel sont presque toujours écrits en C++. Vous n'avez pas besoin de tout maîtriser d'un coup — commencez par l'essentiel et progressez.

## Thèmes

- Pourquoi le C++ ? Vitesse, contrôle de la mémoire, norme de l'industrie robotique
- Types, pointeurs, références
- Flux de contrôle et fonctions
- Structures et classes
- Compilation, édition de liens, systèmes de build (bases de CMake)
- Écriture d'un programme simple de contrôle de moteur en C++

## Code

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

## Action

1. Compilez et exécutez l'exemple ci-dessus.
2. Ajoutez une méthode `stop()` qui met la vitesse à `0.0`.
3. Écrivez une fonction libre `driveForward(Motor& left, Motor& right, float speed)` et appelez-la depuis `main`.
4. Introduisez un pointeur : créez deux objets `Motor` sur le tas avec `new`, utilisez-les, puis libérez-les avec `delete`.

## Réflexion

Après avoir observé le comportement du robot, réfléchissez aux questions suivantes :

- Quelle est la différence entre le passage d'une variable par valeur, par pointeur et par référence ?
- Que se passe-t-il si vous faites un `new` sur un objet sans jamais faire de `delete` ?
- Pourquoi les projets de robotique utilisent-ils CMake au lieu d'appeler `g++` directement ?

## Extension

Modifiez le code C++ pour changer le comportement des moteurs du robot :

1. Changez le rapport cyclique dans `set_speed()` pour utiliser une courbe non linéaire (par exemple, élevez l'entrée au carré) — le profil d'accélération du robot passe de linéaire à progressif.
2. Ajoutez une méthode `reverse()` qui inverse la vitesse actuelle — le robot peut maintenant reculer.
3. Créez deux objets `Motor` et écrivez une fonction qui les fait tourner dans des directions opposées — le robot tourne désormais sur lui-même. Observez la relation entre la structure du code et le mouvement physique.
