# Shader
## GLSL editing
 - It does not have a fixed file name for glsl, but we can use fs or vs for fragment shader and vertex shader respectively

## GLSL structure
1. Declare opengl version
    ```c++
    #version 330 core
    ```
2. Give the port for each attribute, for example like the followings,
    ```c++
    layout(location = 0)
    layout(location = 1)
    ```

3. We need input output from last stage and output for the next stage (shader)
    ```c++
    layout(location = 0) in vec3 aPos; // we get the vertices and pass them to here
    layout(location = 1) in vec3 Color;
    out vec3 our_color; // pass this output to the fragment shader
    ```

4. Each glsl shader is independent of each other, so they have independent main function.

5. Uniform type
    - It is like the global variable in glsl, so we can define it in whatever shader that has been attached in the same program.
    - If the uniform type variable is not defined anywhere, it will be optimized and dumped by the compiler.
    - We can also give it value in cpp by using glUniform function. Example is like the followings,
      ```cpp
      int vertexColorLocation = glGetUniformLocation(Program_ID, uniform_variable.c_str());
      glUniform4f(vertexColorLocation, 0.0f, greenValue, 0.0f, 1.0f);
      ```