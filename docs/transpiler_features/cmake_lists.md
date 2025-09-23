# Configuring CMakeLists.txt

You have some control over the CMakeLists.txt file that the Py++ transpiler writes to your generated C++ project.

If you put a file `.pypp/transpiler_config/cmake_lists.json` in your project:


```json
{
    "add_lines": [
        "include(FetchContent)",
        "FetchContent_Declare(",
        "    glfw",
        "    GIT_REPOSITORY https://github.com/glfw/glfw.git",
        "    GIT_TAG 3.4",
        ")",
        "FetchContent_MakeAvailable(glfw)"
    ],
    "link_libraries": [
        "glfw"
    ]
}
```

then, the Py++ transpiler will put the `add_lines` in the CMakeLists.txt file, and will link the libraries in `link_libraries` to `pypp_common` (all your project executables).

With this `cmake_lists.json` file in your project, your generated CMakeLists.txt will look something like this:

```cmake
cmake_minimum_required(VERSION 4.0)
set(CMAKE_CXX_COMPILER clang++)
set(CMAKE_C_COMPILER clang)
project(pypp LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

include(FetchContent)
FetchContent_Declare(
    glfw
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 3.4
)
FetchContent_MakeAvailable(glfw)

set(SRC_FILES
a_src_file.cpp
)
file(GLOB_RECURSE pypp_FILES pypp/*.cpp)
file(GLOB_RECURSE LIB_FILES libs/*.cpp)

add_library(
    pypp_common STATIC
    ${SRC_FILES}
    ${pypp_FILES}
    ${LIB_FILES}
)
target_include_directories(
    pypp_common PUBLIC
    ${CMAKE_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}/pypp
    ${CMAKE_SOURCE_DIR}/libs
)
target_link_libraries(
    pypp_common PUBLIC
    glfw
)
add_executable(main main.cpp)
target_link_libraries(main PRIVATE pypp_common)
```
