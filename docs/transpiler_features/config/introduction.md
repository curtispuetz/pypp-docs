# Transpiler config introduction

With transpiler config, the Py++ transpiler allows you to configure how it transpiles certain statements and expressions in your code

- This is the feature that is required to enable Py++ to integrate existing C++ libraries without a great amount of effort
- Its usage is primarily intended for Py++ libraries that are integrating C++ libraries; however, you can also use it for a standard Py++ project
    - It is easiest to experiment with and see how it works when you use it in a standard Py++ project
- I have already used this feature to create Py++ libraries for GLFW and OpenGL
- For a regular Py++ project, you put your transpiler config files in a `.pypp/transpiler_config` directory
    - For Py++ libraries, you put your transpiler config files in a `my_lib_name/pypp_data/transpiler_config` directory
        - (I show this clearly in the [OpenGL library example](../../external_libraries/opengl_example.md))

To get an idea of how it works, start by taking a look at the [examples page](examples.md) and then the [better examples page](better_examples.md).
