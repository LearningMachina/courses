# Jeux 3D avec C++

## Concept

Le C++ est le langage des moteurs de jeu professionnels — Unreal Engine, la plupart des titres AAA et le code graphique haute performance sont tous écrits en C++. Vous apprendrez comment le rendu 3D fonctionne depuis la base : sommets, shaders, systèmes de caméra et pipeline GPU. Comprendre cela vous donne un contrôle direct sur ce qui apparaît à l'écran.

## Thèmes

- Pourquoi le C++ pour les jeux 3D ? Performance, contrôle de la mémoire, accès au GPU
- Le pipeline graphique : sommets → vertex shader → rastérisation → fragment shader → écran
- Les bases d'OpenGL : créer une fenêtre avec GLFW, dessiner un triangle
- Sommets, tampons et objets de tableau de sommets (VAOs)
- Shaders : écriture de vertex shaders et fragment shaders en GLSL
- Transformations : matrices de modèle, de vue et de projection
- Systèmes de caméra : caméra à la première personne et caméra orbitale
- Textures : chargement d'images et application sur la géométrie
- Éclairage : modèle d'ombrage de Phong (ambiant, diffus, spéculaire)
- Chargement de modèles 3D : format OBJ et données de maillage

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

## Action

1. Installez les dépendances : `sudo apt install libglfw3-dev libglew-dev`.
2. Créez un fichier `game3d.cpp` avec le code ci-dessus.
3. Compilez avec : `g++ game3d.cpp -o game3d -lGL -lGLEW -lglfw`.
4. Exécutez-le — vous devriez voir une fenêtre vide avec un fond sombre.
5. Ajoutez un triangle coloré en utilisant des objets tampons de sommets et les shaders fournis.
6. Faites tourner le triangle en mettant à jour une matrice de transformation à chaque image.

## Réflexion

- Pourquoi le GPU gère-t-il le rendu au lieu du CPU ?
- Quelle est la différence entre un vertex shader et un fragment shader ?
- Comment la boucle de jeu en C++ se compare-t-elle à la boucle Pygame en Python ?

## Extension

- Ajoutez un cube au lieu d'un triangle — définissez les 36 sommets (6 faces × 2 triangles × 3 sommets).
- Implémentez une caméra simple à la première personne contrôlée par le clavier et la souris.
- Ajoutez un éclairage Phong basique pour que le cube paraisse tridimensionnel.
