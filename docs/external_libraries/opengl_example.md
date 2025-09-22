# Integrating OpenGL to Py++

The following is an example of how to create a Py++ library that integrates OpenGL. Specifcally, it integrates the C++ glad library.


## pyproject.toml

We will start with the pyproject.toml file. Our library name will be `pypp-opengl` and we will put everything in directory `pypp_opengl` (like a standard Python library setup).

```toml
[project]
name = "pypp-opengl"
version = "0.0.0"
description = ""
authors = []
readme = "readme.md"
license = {text = "MIT"}
requires-python = ">=3.13"
dependencies = [
    "numpy",
    "PyOpenGL"
]

[tool.hatch.build]
include = ["pypp_opengl/**/*"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

We have the dependencies `numpy` and `PyOpenGL` because you will see that we actually use those in our libraries Python code.

## cmake_lists.json

Now lets add a file `pypp_opengl/pypp_data/transpiler_config/cmake_lists.json`

```json
{
    "add_lines": [
        "include(FetchContent)",
        "FetchContent_Declare(",
        "    glad",
        "    GIT_REPOSITORY https://github.com/Dav1dde/glad.git",
        "    GIT_TAG v2.0.8",
        "    GIT_PROGRESS TRUE",
        "    SOURCE_SUBDIR cmake",
        ")",
        "FetchContent_MakeAvailable(glad)",
        "glad_add_library(glad_gl_core_43 STATIC REPRODUCIBLE LOADER API gl:core=4.3)"
    ],
    "link_libraries": [
        "glad_gl_core_43"
    ]
}
```

This file will make it so that those lines are added to the generated CMakeLists.txt file for any project that has our library installed.

## Our API

Now lets define our libraries API. Under `pypp_opengl` we will add the following Python files:

```python
# __init__.py
import OpenGL.GL as GL
import numpy as np
from .custom import (
    gl_gen_buffer,
    gl_gen_buffers,
    gl_delete_buffer,
    gl_gen_vertex_array,
    gl_gen_vertex_arrays,
    gl_delete_vertex_array,
    gl_delete_vertex_arrays,
    gl_shader_source,
    gl_shader_sources,
    gl_get_shader_iv,
    gl_get_program_iv,
    gl_get_shader_info_log,
    gl_get_program_info_log,
)
from .glad_loader import glad_load_gl


__all__ = [
    "GL",
    "np",
    "gl_gen_buffer",
    "gl_gen_buffers",
    "gl_delete_buffer",
    "gl_gen_vertex_array",
    "gl_gen_vertex_arrays",
    "gl_delete_vertex_array",
    "gl_delete_vertex_arrays",
    "gl_shader_source",
    "gl_shader_sources",
    "gl_get_shader_iv",
    "gl_get_program_iv",
    "gl_get_shader_info_log",
    "gl_get_program_info_log",
    "glad_load_gl",
]
```

```python
# custom.py
from OpenGL.GL import (
    glGenBuffers,
    glGenVertexArrays,
    GLuint,
    glShaderSource,
    GLenum,
    glGetShaderiv,
    glGetProgramiv,
    glGetShaderInfoLog,
    glGetProgramInfoLog,
    glDeleteBuffers,
    glDeleteVertexArrays,
)


def gl_gen_buffer() -> GLuint:
    return glGenBuffers(1)


def gl_gen_buffers(n: int) -> list[GLuint]:
    return glGenBuffers(n)


def gl_delete_buffer(buffer: GLuint):
    glDeleteBuffers(1, [buffer])


def gl_gen_vertex_array() -> GLuint:
    return glGenVertexArrays(1)


def gl_gen_vertex_arrays(n: int) -> list[GLuint]:
    return glGenVertexArrays(n)


def gl_delete_vertex_array(array: GLuint):
    glDeleteVertexArrays(1, [array])


def gl_delete_vertex_arrays(arrays: list[GLuint]):
    glDeleteVertexArrays(len(arrays), arrays)


def gl_shader_source(shader: GLuint, source: str):
    glShaderSource(shader, source)


def gl_shader_sources(shader: GLuint, sources: list[str]):
    glShaderSource(shader, sources)


def gl_get_shader_iv(shader: GLuint, pname: GLenum) -> int:
    return glGetShaderiv(shader, pname)


def gl_get_program_iv(program: GLuint, pname: GLenum) -> int:
    return glGetProgramiv(program, pname)


def gl_get_shader_info_log(shader: GLuint) -> str:
    return glGetShaderInfoLog(shader)


def gl_get_program_info_log(program: GLuint) -> str:
    return glGetProgramInfoLog(program)
```

```python
# glad_loader.py
def glad_load_gl() -> bool:
    return True
```

With this API, our expectation is that for all OpenGL functions and attributes, users of our library will do:

```python
from pypp_opengl import GL


def pseudo_fn():
    GL.glCreateShader(shader_type)  # example function usage
```

Except for the special ones, they should be imported differently.

```python
from pypp_opengl import GL, gl_gen_buffer

def pseudo_fn():
    vbo: GL.GLuint = gl_gen_buffer()  # example special function usage
```

Its not important for now, but the reason we defined these special ones is because the original API for them was not very compatiable for our purposes.

## Our custom C++ code

Now lets define some custom C++ code that we need to match to the special functions refered to in the last section. Under `pypp_opengl/pypp_data/cpp/pypp_opengl` we will add:

```cpp
// custom.h
#include "py_list.h"
#include "py_str.h"
#include <glad/gl.h>

GLuint gl_gen_buffer();
pypp::PyList<GLuint> gl_gen_buffers(int n);
void gl_delete_buffer(GLuint buffer);
void gl_delete_buffers(pypp::PyList<GLuint> &buffers);
GLuint gl_gen_vertex_array();
pypp::PyList<GLuint> gl_gen_vertex_arrays(int n);
void gl_delete_vertex_array(GLuint array);
void gl_delete_vertex_arrays(pypp::PyList<GLuint> &arrays);
void gl_shader_source(GLuint shader, pypp::PyStr &source);
void gl_shader_sources(GLuint shader, pypp::PyList<pypp::PyStr> &sources);
GLint gl_get_shader_iv(GLuint shader, GLenum pname);
GLint gl_get_program_iv(GLuint program, GLenum pname);
pypp::PyStr gl_get_shader_info_log(GLuint shader);
pypp::PyStr gl_get_program_info_log(GLuint program);
```

```cpp
// custom.cpp
#include "custom.h"

GLuint gl_gen_buffer()
{
    GLuint buffer;
    glGenBuffers(1, &buffer);
    return buffer;
}

pypp::PyList<GLuint> gl_gen_buffers(int n)
{
    pypp::PyList<GLuint> buffers(n);
    glGenBuffers(n, buffers.data_ref().data());
    return buffers;
}

void gl_delete_buffer(GLuint buffer) { glDeleteBuffers(1, &buffer); }

void gl_delete_buffers(pypp::PyList<GLuint> &buffers)
{
    glDeleteBuffers(buffers.len(), buffers.data_ref().data());
}

GLuint gl_gen_vertex_array()
{
    GLuint array;
    glGenVertexArrays(1, &array);
    return array;
}

pypp::PyList<GLuint> gl_gen_vertex_arrays(int n)
{
    pypp::PyList<GLuint> arrays(n);
    glGenVertexArrays(n, arrays.data_ref().data());
    return arrays;
}

void gl_delete_vertex_array(GLuint array) { glDeleteVertexArrays(1, &array); }

void gl_delete_vertex_arrays(pypp::PyList<GLuint> &arrays)
{
    glDeleteVertexArrays(arrays.len(), arrays.data_ref().data());
}

void gl_shader_source(GLuint shader, pypp::PyStr &source)
{
    const char *src = source.str().c_str();
    glShaderSource(shader, 1, &src, nullptr);
}

void gl_shader_sources(GLuint shader, pypp::PyList<pypp::PyStr> &sources)
{
    std::vector<const char *> c_strs;
    c_strs.reserve(sources.len());
    for (int i = 0; i < sources.len(); ++i)
    {
        c_strs.push_back(sources[i].str().c_str());
    }
    glShaderSource(shader, static_cast<GLsizei>(c_strs.size()), c_strs.data(),
                   nullptr);
}

GLint gl_get_shader_iv(GLuint shader, GLenum pname)
{
    int param;
    glGetShaderiv(shader, pname, &param);
    return param;
}

GLint gl_get_program_iv(GLuint program, GLenum pname)
{
    int param;
    glGetProgramiv(program, pname, &param);
    return param;
}

pypp::PyStr gl_get_shader_info_log(GLuint shader)
{
    char infoLog[512];
    glGetShaderInfoLog(shader, 512, nullptr, infoLog);
    return pypp::PyStr(infoLog);
}

pypp::PyStr gl_get_program_info_log(GLuint program)
{
    char infoLog[512];
    glGetProgramInfoLog(program, 512, nullptr, infoLog);
    return pypp::PyStr(infoLog);
}
```

## Our transpiler config

Now, as a last step, we need to add [transpiler config](../transpiler_features/config/introduction.md) which will tell the Py++ transpiler for any projects that have our library installed how to translate certain things.

### Mapping `GL.` usage

First, we will specify `pypp_opengl/transpiler_config/attr_map.json` so that any usage of `GL.glSomeName`, when `GL` is imported as `from pypp_opengl import GL`, is translated to `glSomeName`, with `#include <glad/gl.h>` included:

```json
{
    "custom_mapping_starts_with": {
        "GL.": {
            "mapping_function": "gl_attr.py",
            "angle_includes": [
                "glad/gl.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "GL"
            }
        }
    }
}
```

```python
# pypp_opengl/transpiler_config/mapping_functions/gl_attr.py

def mapping_fn(node: ast.Call, d: Deps, name_str: str) -> str:
    return name_str.split(".")[1]
```

Just so that it is clear: in the Py++ code we write will use `GL.glSomeName`, but in the generated C++ code for users of our library, we need it to be `glSomeName`.

### Mapping calls

Now, we will specify `pypp_opengl/transpiler_config/call_map.json` so that all the special functions we wrote in the Python and C++ code in the above section map to each other properly. For example, when `gl_gen_buffer()` is called, when `gl_gen_buffer` is imported as `from pypp_opengl import gl_gen_buffer`, this will be translated to `gl_gen_buffer()` also, with `#include "pypp_opengl/custom.h"`.

We also have some mapping for the `glad_load_gl` function and for `np.array` (which are other details that we need)

```json
{
    "custom_mapping": {
        "glad_load_gl": {
            "mapping_function": "glad_load_gl.py",
            "angle_includes": [
                "glad/gl.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "glad_load_gl"
            }
        },
        "np.array": {
            "mapping_function": "np_array.py",
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "np"
            }
        }
    },
    "to_string": {
        "gl_gen_buffer": {
            "to": "gl_gen_buffer",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_gen_buffer"
            }
        },
        "gl_gen_buffers": {
            "to": "gl_gen_buffers",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_gen_buffers"
            }
        },
        "gl_delete_buffer": {
            "to": "gl_delete_buffer",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_delete_buffer"
            }
        },
        "gl_delete_buffers": {
            "to": "gl_delete_buffers",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_delete_buffers"
            }
        },
        "gl_gen_vertex_array": {
            "to": "gl_gen_vertex_array",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_gen_vertex_array"
            }
        },
        "gl_gen_vertex_arrays": {
            "to": "gl_gen_vertex_arrays",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_gen_vertex_arrays"
            }
        },
        "gl_shader_source": {
            "to": "gl_shader_source",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_shader_source"
            }
        },
        "gl_shader_sources": {
            "to": "gl_shader_sources",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_shader_sources"
            }
        },
        "gl_get_shader_iv": {
            "to": "gl_get_shader_iv",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_get_shader_iv"
            }
        },
        "gl_get_program_iv": {
            "to": "gl_get_program_iv",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_get_program_iv"
            }
        },
        "gl_get_shader_info_log": {
            "to": "gl_get_shader_info_log",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_get_shader_info_log"
            }
        },
        "gl_get_program_info_log": {
            "to": "gl_get_program_info_log",
            "quote_includes": [
                "pypp_opengl/custom.h"
            ],
            "required_py_import": {
                "module": "pypp_opengl",
                "name": "gl_get_program_info_log"
            }
        }
    }
}
```

```python
# glad_load_gl.py

def mapping_fn(node: ast.Call, d: Deps) -> str:
    return "gladLoadGL(glfwGetProcAddress)"
```

```python
# np_array.py

def mapping_fn(node: ast.Call, d: Deps) -> str:
    assert len(node.args) > 0, "Need at least one arg for np.array"
    return f"{d.handle_expr(node.args[0])}.data_ref().data()"

```

### Always passing by value

Lastly, lets make primitve OpenGL types like GLuint, always pass-by-value for function parameters and class data memebers. `pypp_opengl/transpiler_config/always_pass_by_value.json` 

```json
{
    "GLuint": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLenum": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLboolean": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLbitfield": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLvoid": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLbyte": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLubyte": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLshort": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLushort": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLint": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLclampx": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLsizei": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLfloat": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLclampf": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLdouble": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLclampd": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLchar": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLcharARB": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLhalf": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLhalfARB": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLfixed": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLint64": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    },
    "GLint64EXT": {
        "required_py_import": {
            "module": "pypp_opengl",
            "name": "GL"
        }
    }
}
```

## Summary of project

We have a complete project now. In summary, our files are:

```text
pyproject.toml
pypp_opengl/
    __init__.py
    custom.py
    glad_loader.py
    pypp_data/
        cpp/
            pypp_opengl/
                custom.h
                custom.cpp
        transpiler_config/
            cmake_lists.json
            attr_map.json
            call_map.json
            always_pass_by_value.json
            mapping_functions/
                gl_attr.py
                glad_load_gl.py
                np_array.py

```

## Building our library

Assuming we have `hatchling` installed in our projects virtual environment, we can run the following to build our project: `python -m hatchling build`.

## Distributing our library

Afterign building our project, we can use `twine` or another way to distribute our library to PyPI.

## Installing our library

If the library is on PyPI and named `pypp-opengl`, then we can install it to our project with `pip install pypp-opengl`.

Or, if we just built the project locally and didn't upload it to PyPI, we can install it with `pip install path/to/whl_file.whl`.

## Showcase library usage

This section shows the standard OpenGL example of drawing a triangle with this `pypp-opengl` library we created. Then it shows the generated C++ code that the Py++ transpiler generates. This showcases everything we worked on in the above sections, especially in the `transpiler_config`.

In order to work with OpenGL, we also need a window management library. So, we are using a Py++ `pypp-glfw` library in this example as well.

```python
# draw_triangle.py
from pypp_opengl import (
    gl_gen_buffer,
    gl_gen_vertex_array,
    gl_shader_source,
    gl_get_shader_iv,
    gl_get_program_iv,
    gl_get_shader_info_log,
    gl_get_program_info_log,
    gl_delete_buffer,
    gl_delete_vertex_array,
    glad_load_gl,
    np,
    GL,
)
from pypp_glfw import GLFWwindowPtr, glfw
from pypp_python import to_c_string, NULL, float32
from pypp_python.stl import ctypes

# Vertex shader source
vertex_shader_src: str = """
#version 330 core
layout(location = 0) in vec3 position;
layout(location = 1) in vec3 color;

out vec3 vertexColor;

void main()
{
    gl_Position = vec4(position, 1.0);
    vertexColor = color;
}
"""

# Fragment shader source
fragment_shader_src: str = """
#version 330 core
in vec3 vertexColor;
out vec4 FragColor;

void main()
{
    FragColor = vec4(vertexColor, 1.0);
}
"""


def compile_shader(source: str, shader_type: GL.GLenum) -> GL.GLuint:
    shader: GL.GLuint = GL.glCreateShader(shader_type)
    gl_shader_source(shader, source)
    GL.glCompileShader(shader)
    if not gl_get_shader_iv(shader, GL.GL_COMPILE_STATUS):
        raise RuntimeError(
            "Shader compilation failed: " + gl_get_shader_info_log(shader)
        )
    return shader


def create_shader_program() -> GL.GLuint:
    vertex_shader: GL.GLuint = compile_shader(vertex_shader_src, GL.GL_VERTEX_SHADER)
    fragment_shader: GL.GLuint = compile_shader(
        fragment_shader_src, GL.GL_FRAGMENT_SHADER
    )

    program: GL.GLuint = GL.glCreateProgram()
    GL.glAttachShader(program, vertex_shader)
    GL.glAttachShader(program, fragment_shader)
    GL.glLinkProgram(program)

    if not gl_get_program_iv(program, GL.GL_LINK_STATUS):
        raise RuntimeError(
            "Program linking failed: " + gl_get_program_info_log(program)
        )

    GL.glDeleteShader(vertex_shader)
    GL.glDeleteShader(fragment_shader)

    return program


def opengl_test():
    # Initialize GLFW
    if not glfw.init():
        raise Exception("Failed to initialize GLFW")

    glfw.window_hint(glfw.CONTEXT_VERSION_MAJOR, 4)
    glfw.window_hint(glfw.CONTEXT_VERSION_MINOR, 6)
    glfw.window_hint(glfw.OPENGL_PROFILE, glfw.OPENGL_CORE_PROFILE)

    # Create window
    window: GLFWwindowPtr = glfw.create_window(
        800, 600, to_c_string("PyOpenGL Triangle"), NULL, NULL
    )
    if not window:
        glfw.terminate()
        raise Exception("Failed to create GLFW window")

    glfw.make_context_current(window)

    if not glad_load_gl():
        raise Exception("Failed to initialize GLAD")

    # Vertex data (positions + colors)
    # fmt: off
    vertices: list[float32] = [
        -0.5, -0.5, 0.0, 
        1.0, 0.0, 0.0,  # bottom left (red)
        0.5, -0.5, 0.0,
        0.0, 1.0, 0.0,  # bottom right (green)
        0.0, 0.5, 0.0,
        0.0, 0.0, 1.0,  # top (blue)
    ]
    # fmt: on

    # Create VAO and VBO
    vao: GL.GLuint = gl_gen_vertex_array()
    vbo: GL.GLuint = gl_gen_buffer()

    GL.glBindVertexArray(vao)

    GL.glBindBuffer(GL.GL_ARRAY_BUFFER, vbo)
    GL.glBufferData(
        GL.GL_ARRAY_BUFFER,
        len(vertices) * GL.sizeof(GL.GLfloat),
        np.array(vertices, np.float32),
        GL.GL_STATIC_DRAW,
    )

    # Position attribute
    GL.glVertexAttribPointer(
        0, 3, GL.GL_FLOAT, GL.GL_FALSE, 6 * GL.sizeof(GL.GLfloat), ctypes.c_void_p(0)
    )
    GL.glEnableVertexAttribArray(0)

    # Color attribute
    GL.glVertexAttribPointer(
        1,
        3,
        GL.GL_FLOAT,
        GL.GL_FALSE,
        6 * GL.sizeof(GL.GLfloat),
        ctypes.c_void_p(3 * GL.sizeof(GL.GLfloat)),
    )
    GL.glEnableVertexAttribArray(1)

    # Build shader program
    shader_program: GL.GLuint = create_shader_program()

    # Main render loop
    while not glfw.window_should_close(window):
        glfw.poll_events()

        GL.glClearColor(0.2, 0.3, 0.3, 1.0)
        GL.glClear(GL.GL_COLOR_BUFFER_BIT)

        GL.glUseProgram(shader_program)
        GL.glBindVertexArray(vao)
        GL.glDrawArrays(GL.GL_TRIANGLES, 0, 3)

        glfw.swap_buffers(window)

    # Cleanup
    gl_delete_vertex_array(vao)
    gl_delete_buffer(vbo)
    glfw.terminate()


if __name__ == "__main__":
    opengl_test()
```

this translates to a .cpp file:

```cpp
#include "cstdlib"
#include "exceptions/common.h"
#include "exceptions/exception.h"
#include "py_list.h"
#include "py_str.h"
#include "pypp_opengl/custom.h"
#include "pypp_util/main_error_handler.h"
#include <GLFW/glfw3.h>
#include <glad/gl.h>

pypp::PyStr vertex_shader_src =
    pypp::PyStr("\n#version 330 core\nlayout(location = 0) in vec3 "
                "position;\nlayout(location = 1) in vec3 color;\n\nout vec3 "
                "vertexColor;\n\nvoid main()\n{\n    gl_Position = "
                "vec4(position, 1.0);\n    vertexColor = color;\n}\n");
pypp::PyStr fragment_shader_src = pypp::PyStr(
    "\n#version 330 core\nin vec3 vertexColor;\nout vec4 FragColor;\n\nvoid "
    "main()\n{\n    FragColor = vec4(vertexColor, 1.0);\n}\n");
GLuint compile_shader(pypp::PyStr &source, GLenum shader_type) {
    GLuint shader = glCreateShader(shader_type);
    gl_shader_source(shader, source);
    glCompileShader(shader);
    if (!gl_get_shader_iv(shader, GL_COMPILE_STATUS)) {
        throw pypp::RuntimeError(pypp::PyStr("Shader compilation failed: ") +
                                 gl_get_shader_info_log(shader));
    }
    return shader;
}

GLuint create_shader_program() {
    GLuint vertex_shader = compile_shader(vertex_shader_src, GL_VERTEX_SHADER);
    GLuint fragment_shader =
        compile_shader(fragment_shader_src, GL_FRAGMENT_SHADER);
    GLuint program = glCreateProgram();
    glAttachShader(program, vertex_shader);
    glAttachShader(program, fragment_shader);
    glLinkProgram(program);
    if (!gl_get_program_iv(program, GL_LINK_STATUS)) {
        throw pypp::RuntimeError(pypp::PyStr("Program linking failed: ") +
                                 gl_get_program_info_log(program));
    }
    glDeleteShader(vertex_shader);
    glDeleteShader(fragment_shader);
    return program;
}

void opengl_test() {
    if (!glfwInit()) {
        throw pypp::Exception(pypp::PyStr("Failed to initialize GLFW"));
    }
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 6);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
    GLFWwindow *window = glfwCreateWindow(
        800, 600, pypp::PyStr("PyOpenGL Triangle").str().c_str(), NULL, NULL);
    if (!window) {
        glfwTerminate();
        throw pypp::Exception(pypp::PyStr("Failed to create GLFW window"));
    }
    glfwMakeContextCurrent(window);
    if (!gladLoadGL(glfwGetProcAddress)) {
        throw pypp::Exception(pypp::PyStr("Failed to initialize GLAD"));
    }
    pypp::PyList<float> vertices({-0.5, -0.5, 0.0, 1.0, 0.0, 0.0, 0.5, -0.5,
                                  0.0, 0.0, 1.0, 0.0, 0.0, 0.5, 0.0, 0.0, 0.0,
                                  1.0});
    GLuint vao = gl_gen_vertex_array();
    GLuint vbo = gl_gen_buffer();
    glBindVertexArray(vao);
    glBindBuffer(GL_ARRAY_BUFFER, vbo);
    glBufferData(GL_ARRAY_BUFFER, vertices.len() * sizeof(GLfloat),
                 vertices.data_ref().data(), GL_STATIC_DRAW);
    glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(GLfloat),
                          (void *)(0));
    glEnableVertexAttribArray(0);
    glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(GLfloat),
                          (void *)(3 * sizeof(GLfloat)));
    glEnableVertexAttribArray(1);
    GLuint shader_program = create_shader_program();
    while (!glfwWindowShouldClose(window)) {
        glfwPollEvents();
        glClearColor(0.2, 0.3, 0.3, 1.0);
        glClear(GL_COLOR_BUFFER_BIT);
        glUseProgram(shader_program);
        glBindVertexArray(vao);
        glDrawArrays(GL_TRIANGLES, 0, 3);
        glfwSwapBuffers(window);
    }
    gl_delete_vertex_array(vao);
    gl_delete_buffer(vbo);
    glfwTerminate();
}

int main() {
    try {
        opengl_test();
        return 0;
    } catch (...) {
        pypp::handle_fatal_exception();
        return EXIT_FAILURE;
    }
}
```

## Conclusion

See [OpenGL Example Conclusion](opengl_example_conclusion.md)