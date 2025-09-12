# Learn Py++: An introduction to writing Py++ code

This page is an introduction to writing Py++ code.

I assume the reader has a little knowledge of C++.

## Main files and Src files

Python files that you put in the first level of the project 'python' directory are main files (i.e. python/my_main_file.py), and Python files that you put in the 'python/src' directory are src files (i.e. python/src/my_src_file.py).

Main files must have a Python main block:

```python
if __name__ == "__main__":
    # Your code goes here
```

because each main file is transpiled to a .cpp file that has a main function:

```cpp
#include "cstdlib"
#include "pypp_util/main_error_handler.h"

int main() {
    try {
        // Your code, transpiled
        return 0;
    } catch (...) {
        pypp::handle_fatal_exception();
        return EXIT_FAILURE;
    }
}
```

Therefore, for each main file in your project, cmake will generate an executable that you can run.

Src files on the other hand, should not have a main block. They are transpiled to a .h file, and in most cases a .cpp file as well.

## Type hints

You must use Python-style type hints everywhere. I.e. for variable definitions, function parameters, class data members, and return types.

## Memory management

At this point, you should read the page on [memory management](lang_features/manual_memory_management.md). Then, if you want, come back and look at the examples below, which show how the Py++ transpiler works to translate your code to C++.

## Examples

I am going to show some example Py++ code, and then show what C++ code it transpiles into. For people with an understanding of C++, this will help show why Py++ works the way it does.

You will see that generally in Py++ each statement/expression translates 1-to-1 to a statement/expression in the generated C++ code.

### 1) A function

If you add the following function to a Py++ src file

```python
# list_adder.py
def list_add(a: list[int], b: list[int], mult_factor: int) -> list[int]:
    assert len(a) == len(b), "List lengths should be equal"
    ret: list[int] = []
    for i in range(len(a)):
        ret.append(mult_factor *(a[i] + b[i]))
    return ret
```

this will transpile to C++ .h and .cpp files:

```cpp
// list_adder.h
#pragma once

#include "py_list.h"

namespace me {
pypp::PyList<int> list_add(pypp::PyList<int> &a, pypp::PyList<int> &b,
                           int mult_factor);
}
```

```cpp
// list_adder.cpp
#include "list_adder.h"
#include "py_str.h"
#include "pypp_assert.h"

namespace me {
pypp::PyList<int> list_add(pypp::PyList<int> &a, pypp::PyList<int> &b,
                           int mult_factor) {
    pypp::assert(a.len() == b.len(),
                 pypp::PyStr("List lengths should be equal"));
    pypp::PyList<int> ret({});
    for (int i = 0; i < a.len(); i += 1) {
        ret.append(mult_factor * (a[i] + b[i]));
    }
    return ret;
}
}
```

- You can see that the types `list` and `str` in the Py++ code translate to `pypp::PyList` and `pypp::PyStr`
    - `pypp::PyList` and `pypp::PyStr` are thin wrappers around `std::vector` and `std::string` respectively
    - Another note: Not shown in this example, but there is also `pypp::PyDict` and `pypp::PySet`, for the Py++ `dict` and `set` types, which thinly wrap `std::unordered_map` and `std::unordered_set`, respectively
- You can see that the C++ code is wrapped in a `me` namespace
    - All Py++ src files you write are translated to C++ files wrapped in a `me` namespace

### 2) Union and Optional types

If you are used to using Python's `isinstance()` function to check the type of an object, you can do something very similar in Py++ with `isinst()`.

If you add the following function to a Py++ src file

```python
# union_example.py
from pypp_python import Uni, ug, isinst, is_none


def union_example():
    # Union of int, float, and list
    int_float_or_list: Uni[int, float, list[int]] = Uni(3.14)
    if isinst(int_float_or_list, float):
        val: float = ug(int_float_or_list, float)
        print(val)
    # Union with None (i.e. like an Optional)
    b: Uni[int, None] = Uni(None)
    if is_none(b):
        print("b is None")
```

this will transpile to C++ .h and .cpp files:

```cpp
// union_example.h
#pragma once

namespace me {
void union_example();
}
```

```cpp
// union_example.cpp
#include "union_example.h"
#include "py_list.h"
#include "py_str.h"
#include "pypp_union.h"
#include "pypp_util/print.h"

namespace me {
void union_example() {
    pypp::Uni<int, double, pypp::PyList<int>> int_float_or_list(3.14);
    if (int_float_or_list.isinst<double>()) {
        double val = int_float_or_list.ug<double>();
        pypp::print(val);
    }
    pypp::Uni<int, std::monostate> b(std::monostate{});
    if (b.is_none()) {
        pypp::print(pypp::PyStr("b is None"));
    }
}
}
```

- You can see that the `Uni` type translates to `pypp::Uni`
    - `pypp::Uni` is a thin wrapper around `std::variant`
- You can see that functions `isinst()` (i.e. isinstance), and `is_none()`, are used to check the type
- You can see that `ug()` (i.e. union get), is used to get the actual value

### 3) Classes

If you add the following class to a Py++ src file

```python
# greeter.py
from dataclasses import dataclass


@dataclass
class Greeter:
    name: str
    prefix: str

    def greet(self) -> str:
        return f"Hello, {self.prefix} {self.name}!"
```

this will transpile to C++ .h and .cpp files:

```cpp
// greeter.h
#pragma once

#include "py_str.h"

namespace me {
struct Greeter {
    pypp::PyStr &name;
    pypp::PyStr &prefix;
    Greeter(pypp::PyStr &a_name, pypp::PyStr &a_prefix)
        : name(a_name), prefix(a_prefix) {}
    pypp::PyStr greet();
};
}
```

```cpp
// greeter.cpp
#include "greeter.h"

namespace me {
pypp::PyStr Greeter::greet() {
    return pypp::PyStr(std::format("Hello, {} {}!", prefix, name));
}
}
```

- To define a class in Py++, you must use the `@dataclass` annotation (except for interfaces, config classes, and custom exception types)
    - You can use `frozen` or `slots` options if you want
        - if you use the `frozen` option, then the generated C++ struct will use `const` for the instance variables
- In Py++, you cannot put logic in a constructor
    - Instead, you can use the factory function pattern
