# Pure library example

The following is an example of how to create a Pure Py++ library.

## pyproject.toml

We will start with the pyproject.toml file. Our library name will be `pypp-pure-library-test-0` and we will put our code in `pypp_pure_library_test_0` (like a standard Python library setup).

```toml
[project]
name = "pypp-pure-library-test-0"
version = "0.0.0"
description = ""
requires-python = ">=3.13"
dependencies = [
    "pypp-python"
]

[tool.hatch.build]
include = [
    "pypp_pure_library_test_0/**/*", 
]
exclude = [
    "pypp_pure_library_test_0/pypp_data/cpp/CMakeLists.txt",
    "pypp_pure_library_test_0/pypp_data/cpp/.clang-format",
    "pypp_pure_library_test_0/pypp_data/cpp/pypp/**/*",
    "pypp_pure_library_test_0/pypp_data/cpp/build/**/*"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

You can see that we added the pypp-python dependency (because its useful to have this for all Py++ projects), and that we are excluding some things in the `cpp` directory from the build. The things that we are excluding we don't want to be packaged into our library. They are written to the `cpp` directory by the Py++ transpiler so that we can make sure our libraries generated C++ code builds without errors, but we don't want to package them into our library.

## .pypp/proj_info.json

Next, we should add a `.pypp/proj_info.json` file to our project:

```json
{
    "cpp_dir_is_dirty": false,
    "namespace": "pure_test_0",
    "override_cpp_write_dir": "pypp_pure_library_test_0/pypp_data/cpp",
    "write_metadata_to_dir": "pypp_pure_library_test_0/pypp_data",
    "ignored_files": [],
    "cmake_minimum_required_version": "4.0"
}
```

We pick a namespace `pure_test_0` for our library, so that our names do not interfer with the code of our libraries users.

We need to specify `override_cpp_write_dir` and `write_metadata_to_dir` to those specific `pypp_data/cpp` and `pypp_data` directories. These are the locations where the Py++ transpiler will write the C++ code to and where it will write a metadata file to, respectively. We need to do this because we need to package those things into our library in those specific locations for distribution.

## Writing our library code

That is all the configuration we need. Now we can just write our Py++ code. Lets say we want our library to just have a function to add two integers (just an easy example):

```python
# pypp_pure_library_test_0/add_ints.py

def add(a: int, b: int) -> int:
    return a + b
```

and we add this to an `__init__.py` file in the normal way for Python libraries

```python
# pypp_pure_library_test_0/__init__.py
from .add_ints import add

__all__ = ["add"]
```

## Transpiling our library

After we have written our code, we use the Py++ transpiler to generate the C++ code.

```text
pypp do transpile format
```

This will write C++ code to the `pypp_pure_library_test_0/pypp_data/cpp` directory

## Building the generated C++ project

To make sure that we have not made any mistakes, we should build the C++ project to confirm there are no compilation errors. You can navigate to the `pypp_pure_library_test_0/pypp_data/cpp` directory and do `CMake` commands, or if you have `clang` installed you can do

```text
pypp do build
```

## Summary of project

We have a complete project now. In summary, our files are:

```text
pyproject.toml
.pypp/
    proj_info.json
pypp_pure_library_test_0/
    __init__.py
    add_ints.py
    pypp_data/
        # generated directory:
        cpp/
```

## packaging and distributing our library

After we have made sure our C++ code builds without compilation errors, we build the library like a typical Python library with

```text
python -m hatchling build
```

Now we can distribute it to PyPI or github

## Using our library in a project

After the library is packaged and distributed, we can install it to our Py++ project with `pip` as described in [installing a Py++ library to your project](introduction.md#installing-a-py-library-to-your-project):

```text
pip install pypp-pure-library-test-0
```

Or, if it is not distributed and just built locally

```text
pip install path/to/whl_file.whl
```

Thats it, now we can use our library within our Py++ project:

```python
from pypp_pure_library_test_0 import add


def pseudo_fn():
    result: int = add(1, 2)
    print(result)
```