# 3D Games with C++

## Concept

C++ is the language of professional game engines — Unreal Engine, most AAA titles, and high-performance graphics code are all written in C++. You will learn how 3D rendering works from the ground up: vertices, shaders, camera systems, and the GPU pipeline. Understanding this gives you direct control over what appears on screen.

## Topics

- Why C++ for 3D games? Performance, memory control, GPU access
- The graphics pipeline: vertices → vertex shader → rasterisation → fragment shader → screen
- OpenGL basics: creating a window with GLFW, drawing a triangle
- Vertices, buffers, and vertex array objects (VAOs)
- Shaders: writing GLSL vertex and fragment shaders
- Transformations: model, view, and projection matrices
- Camera systems: first-person and orbit cameras
- Textures: loading images and mapping them onto geometry
- Lighting: Phong shading model (ambient, diffuse, specular)
- Loading 3D models: OBJ format and mesh data

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

1. Install the dependencies: `sudo apt install libglfw3-dev libglew-dev`.
2. Create a file `game3d.cpp` with the code above.
3. Compile with: `g++ game3d.cpp -o game3d -lGL -lGLEW -lglfw`.
4. Run it — you should see a blank window with a dark background.
5. Add a coloured triangle using vertex buffer objects and the shaders provided.
6. Make the triangle rotate by updating a transformation matrix each frame.

## Reflection

- Why does the GPU handle rendering instead of the CPU?
- What is the difference between a vertex shader and a fragment shader?
- How does the game loop in C++ compare to the Python Pygame loop?

## Extension

- Add a cube instead of a triangle — define all 36 vertices (6 faces × 2 triangles × 3 vertices).
- Implement a simple first-person camera controlled by keyboard and mouse.
- Add basic Phong lighting so the cube looks three-dimensional.
