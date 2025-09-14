# Creating Tiangle

## Structure Note
1. glfw handles the window related features
	 ``` c++
   //initialize the glfw library
   glfwInit();
   // Expect the major version to be 3
   glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
   // Expect the minor version to be 3
   glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
   // Expect to use modern core features
   glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
	```
2. create a window by glfw
	```C++
	GLFWwindow* window = glfwCreateWindow(width, height, "name of the window", NULL);
	```

3.  ```c++
	gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)
	```
	This is how we initialize glad. Ask GLFW f0r all addresses of OpenGL functions, then let glad to load them.


## Three buffer types
1. VBO (vertex buffer object)
2. VAO (vertex array object)
3. EBO (element buffer object)

   ### VBO
         Note: we can think openGL as a warehouse
      1. First, request a code for this buffer from OpenGl. **(glGenBuffers(1, &VBO))**
      2. Tell openGL type of this buffer by **(GL_ARRAY_BUFFER, VBO)**.
      3. Assign the space in GPU. **(glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW))**
         - GL_STATIC_DRAW: put the buffer once and mostly leave it alone
         - GL_DYNAMIC_DRAW: expect to modify the buffer data frequently
         - GL_STREAM_DRAW: change the data every frame.
   ### VAO
      1. VAO stores how vertex related attributes are stored and draw from gpu
      2. We create the ID of VAO and generate VAO array in gpu. **glGenVertexArrays(1, &VAO);**
      3. We bind the VAO generated and all further attributes states are gonna be recorded in this VAO. **glBindVertexArray(VAO);**
      4.  ```c++
            glVertexAttribPointer(slot_pos, numberofVertices, GL_FLOAT, GL_FALSE);
            glEnableVertexAttriArray(0);
          ```

    ### EBO
       1. A buffer that stores indices that OpenGL uses to decide what vertices to draw.
       2. We bind EBO with GL_ELEMENT_ARRAY_BUFFER, which will make it store in VAO.
       ```c++
       unsigned int EBO;
       glGenBuffers(1, &EBO);
       glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
       ```
       3. Similar to VBO, we create actual storage inside GPU through the following,
       ```c++
      glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC);
       ```

## How to create a shader
1. Vertex Shader
2. Fragment Shader
3. Program
   ### Vertex Shader
   1. create an id **unsigned int vertex_shader = glCreateShader(GL_VERTEX_SHADER);**
   2. Use GLSL to get the way to parse the vertex
         ```C++
         const char* vertexshadersource = "#version 330 core\n"
               "layout(location = 0) in vec3 aPos;\n"
               "void main()\n"
               "{\n"
               "  gl_position = vec4f(aPos.x, aPos.y, aPos.z, 1);\n"
               "}\0";
         ```
   3. Send glsl code to GPU **glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);**
   4. Compile the Shader **glCompileShader(vertexShader);**

   ## Fragment Shader
   5. Create an id **unsigned int fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);**
   6. glsl to get a way to parse the fragement color
         ```C++
         const char*fragmentShaderSource = "#version 330 core\n"
         "out vec4 FragColor; \n"
         "void main()\n"
         "{\n"
         "  FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f);\n"
         "}\n\0";
         ```
   7. Send GLSL to GPU **glShaderSource(fragment);**
   8. Compile the fragment shader **glCompileShader(frgamentShader);**

   ## Program (Link vertex shader and fragment shader together)
   9. create program ID **unsigned int shaderProgram = glCreateProgram()**
   10. Attach the shader and program together, then link all the shaders.
      ```c++
      glAttachShader(shaderProgram, vertexShader);
      glAttachShader(shaderProgram, fragmentShader);
      glLinkProgram(shaderProgram);
      ```

## Run the overall process
```c++
glPolygonMode(GL_FRONT_AND_BACK, GL_LINE);
while(!glfwWindowShouldClose(window)){
      processInput(window);

      glClearColor(0.2f, 0.3f, 0.3f, 1.0f); // update the background color in each round
      glBindVertexArray(VAO);
      // Third argument is the type of the indices.
      glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);  // We are drawing a rectangle in here

      glfwSwapBuffers(windows); // we can think of glfw window has two layers, we draw the latter one and swap that with the front one, and then show it on the screen.
      glfwPollEvents(); // execute all the events waiting on the queue. (mouse movement, window resize, keyboard input, etc.)
}
```
