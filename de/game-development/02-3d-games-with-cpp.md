# 3D-Spiele mit C++

## Konzept

C++ ist die Sprache professioneller Game Engines — Unreal Engine, die meisten AAA-Titel und Hochleistungs-Grafikcode sind alle in C++ geschrieben. Du lernst, wie 3D-Rendering von Grund auf funktioniert: Vertices, Shader, Kamerasysteme und die GPU-Pipeline. Dieses Verständnis gibt dir direkte Kontrolle über das, was auf dem Bildschirm erscheint.

## Themen

- Warum C++ für 3D-Spiele? Performance, Speicherkontrolle, GPU-Zugriff
- Die Grafik-Pipeline: Vertices → Vertex-Shader → Rasterisierung → Fragment-Shader → Bildschirm
- OpenGL-Grundlagen: Fenster mit GLFW erstellen, ein Dreieck zeichnen
- Vertices, Buffer und Vertex Array Objects (VAOs)
- Shader: GLSL Vertex- und Fragment-Shader schreiben
- Transformationen: Model-, View- und Projektionsmatrizen
- Kamerasysteme: Ego-Perspektive und Orbit-Kameras
- Texturen: Bilder laden und auf Geometrie abbilden
- Beleuchtung: Phong-Shading-Modell (ambient, diffus, spekular)
- 3D-Modelle laden: OBJ-Format und Mesh-Daten

## Code

```cpp
#include <GL/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>

// Vertex shader source
const char* vertexShaderSrc = R"(
    #version 330 core
    layout (location = 0) in vec3 aPos;
    layout (location = 1) in vec3 aColor;
    out vec3 vertexColor;
    uniform mat4 transform;
    void main() {
        gl_Position = transform * vec4(aPos, 1.0);
        vertexColor = aColor;
    }
)";

// Fragment shader source
const char* fragmentShaderSrc = R"(
    #version 330 core
    in vec3 vertexColor;
    out vec4 FragColor;
    void main() {
        FragColor = vec4(vertexColor, 1.0);
    }
)";

int main() {
    glfwInit();
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);

    GLFWwindow* window = glfwCreateWindow(800, 600, "3D Game", nullptr, nullptr);
    glfwMakeContextCurrent(window);
    glewInit();

    // Game loop
    while (!glfwWindowShouldClose(window)) {
        glfwPollEvents();
        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
        // Draw scene here
        glfwSwapBuffers(window);
    }

    glfwTerminate();
    return 0;
}
```

## Aktion

1. Installiere die Abhängigkeiten: `sudo apt install libglfw3-dev libglew-dev`.
2. Erstelle eine Datei `game3d.cpp` mit dem obigen Code.
3. Kompiliere mit: `g++ game3d.cpp -o game3d -lGL -lGLEW -lglfw`.
4. Führe es aus — du solltest ein leeres Fenster mit dunklem Hintergrund sehen.
5. Füge ein farbiges Dreieck mit Vertex Buffer Objects und den bereitgestellten Shadern hinzu.
6. Lass das Dreieck rotieren, indem du in jedem Frame eine Transformationsmatrix aktualisierst.

## Reflexion

- Warum übernimmt die GPU das Rendering anstelle der CPU?
- Was ist der Unterschied zwischen einem Vertex-Shader und einem Fragment-Shader?
- Wie vergleicht sich die Spielschleife in C++ mit der Python-Pygame-Schleife?

## Erweiterung

- Füge einen Würfel anstelle eines Dreiecks hinzu — definiere alle 36 Vertices (6 Flächen × 2 Dreiecke × 3 Vertices).
- Implementiere eine einfache Ego-Kamera, die mit Tastatur und Maus gesteuert wird.
- Füge einfache Phong-Beleuchtung hinzu, damit der Würfel dreidimensional wirkt.
