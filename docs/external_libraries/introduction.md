# External libraries introduction

Py++ supports external libraries and uses [`pip`](https://en.wikipedia.org/wiki/Pip_(package_manager)) to make library management easy.

## Installing a Py++ library to your project

In order to use external libraries, you need to have a Python virtual environment installed in your Py++ project directory and the name of the virtual environment directory must be `.venv`. You can create that with this exact command:

```text
python -m venv .venv
```

When you install external libraries with `pip`, you should install them to this virtual environment. For example, with the virtual environment activated:

```text
pip install pypp-opengl
```

## Creating a Pure Py++ library

In general, to create a Pure Py++ library, you follow the same process as creating a Pure Python library:

- write your library code (Python files)
- add a pyproject.toml file
- build your project with something like [`hatchling`](https://pypi.org/project/hatchling/)
- If you want, upload your project to PyPI or github for distribution

The extra thing you need to do however, is to transpile your code with the Py++ transpiler, and make sure the generated C++ code is packaged into your library when you build it. Specifically, the C++ code needs to be at `my_lib_name/pypp_data/cpp`. On [this page](pure_library_example.md), is an example of creating and using a Pure Py++ library.

## How installed libraries work

You install Py++ libraries to your project with pip. Then, each time you transpile your Py++ code, the transpiler looks through your `site-packages` directory of your Python virtual environment to see if any of the packages match a Py++ project structure. If it finds any Py++ libraries, then it copies the libraries C++ code to your projects C++ directory (if it has not already done so in a previous transpile).

## Creating a Py++ library that integrates with existing C++ libraries

The following might come across as confusing. And, it maybe is truly a little confusing, but once you get the hang of it, you can really integrate existing C++ libraries to the Py++ ecosystem easily and without great effort. The [example of integrating OpenGL](opengl_example.md) might help a lot to make this less confusing, and the [conclusions](opengl_example_conclusion.md) from that example will help explain what is challenging and why it does not take great effort.

As a primer to this topic, think about this: in the process of creating a Pure Py++ library that I described above, there is nothing stopping you from modifying the generated C++ code after running the Py++ transpiler, so that your modified code is actually what is shipped at the end of the day. You can do this if you want, because perhaps you want to use features in C++ that you can't in the Py++ code. But, more pertaining to the topic of integrating existing C++ libraries, you can also forget about using the Py++ transpiler completely, and write your Python code separately and write your C++ separately.

### If you have access to the C++ libraries source code

If you want to integrate an existing C++ library, and you have access to the C++ libraries source code, you can copy the source code to your Py++ libraries C++ directory, and then write your Python code (which defines the API for your library). You can implement the functionality also in your Python code so that your Py++ library works for users when running via the C++ executable and via the Python interpreter. Or, you can just define the API in the Python code and put `pass` in everything instead of actual implementation. If you do this option, you can document your library with a [`🚫noPyI` symbol](nopyi_libraries.md) to indicate that users can't run their code with the Python interpreter when using your library, and only running the usual way with the C++ generated executable will work.

### If you can integrate the C++ library just through a CMakeLists.txt file

If you want to integrate an existing C++ library that can be integrated to a C++ project just through a CMakeLists.txt file, it is even easier to do. You can add to your Py++ library a `cmake_lists.json` file which tells the Py++ transpiler for projects that have your library installed what should be added to the generated CMakeLists.txt file. Its best to look at the [OpenGL example](opengl_example.md#cmake_listsjson) to see how that works.

### Ensuring the Py++ transpiler knowns how to transpile code from your Py++ library

There is, of course, another step in the process of integrating an existing C++ library. If you define for your library the API

```python
# __init__.py


def gsl_sf_bessel_J0():
    pass
```

then for a project with this Py++ library installed, the Py++ transpiler needs to know what to do when the user uses the function `gsl_sf_bessel_J0`. The corresponding C++ function that you want the generated C++ code to call could be named the same name or a different name. If it is named different, you need to specify in your Py++ library what the different name is and what `#include` statement should be put in the generated C++ file, and if it is named the same, you still need to specify in your library the `#include` statement. This can be done with the Py++ [transpiler config feature](../transpiler_features/config/introduction.md)

How to do this is shown extensively in the [OpenGL example](opengl_example.md#our-transpiler-config).