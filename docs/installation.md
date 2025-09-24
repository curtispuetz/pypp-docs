# Installation

Install Python from python.org, then install the CLI via `pip`:

`pip install pypp-cli`

It is recommended that you install `pypp-cli` globally and have it on your path so that you can use the CLI anywhere on your computer.

To build your generated C++ project, you will need `CMake` installed.

Also, to build your generated C++ project, you will need a C++ compiler installed. The compiler that I recommend, because it is the one I am doing the most testing with, is `clang/clang++`. However, any C++ compiler should work, there is only a slightly greater chance that you will hit a Py++ bug with other compilers.

If you are using `clang`, you will also need `ninja` installed. This can be installed via `pip`:

`pip install ninja`

If you want to format your generated C++ code, you will need `clang-format` installed. If you already have the `clang` compiler installed, you likely have `clang-format` already (another reason why I recommend `clang`). If not, you can install `clang-format` separately.


## Compatible versions

- Python - 3.13.7
- CMake - 4.1.1
- clang - 21.1.0


## Testing your installation by running 'hello world'

To test your installation by running a hello world program, there are only 2 steps. First, initialize a Py++ project in your current directory:

`pypp init`

This gives you a main.py file with a hello world program. Second, if you are using the `clang` compiler, you can `transpile`, `format`, `build`, and `run` the `main` executable with the command:

`pypp do transpile format build run -e main`

This should print the 'hello world' message to the terminal.

### If you are not using `clang`

If you are not using `clang`, you can do `pypp do transpile format`, and then do the required CMake commands in the C++ directory (`.pypp/cpp`) yourself and run the executable.