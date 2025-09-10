# Introduction to writing Py++ code

This is an introduction to writing Py++ code, and is how I would introduce it on the first day of a class if I was teaching a course on Py++. All other required information to be able to write Py++ code, in is the future sections on the languages features and standard library.

This introduction will be better understood by those who have a little knowledge of C++ and Python. Without knowledge of these, it might be a little challenging to understand.

## Main files and Src files

Python files that are in the first level of the project 'python' directory are main files (i.e. python/my_main_file.py), and Python files that are in the 'python/src' directory are src files (i.e. python/src/my_src_file.py).

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

So, for each main file in your project, cmake will generate an executable.

Src files on the other hand, should not have a main block. They are transpiled to a .h file, and in most cases a .cpp file as well.

## Some rules and conventions of writing Py++ code
- You must use Python-style type hints pretty much everywhere
- Parameters are pass-by-reference by default
    - If you want to use pass-by-value, you wrap the type with 'Valu()'
- Return types are return-by-value by default
    - If you want to return-by-reference, you wrap the type with 'Ref()'
- If you want to move ownership of data (i.e. the C++ std::move function), then you use 'mov()'
- You can't have any logic in class contructors
    - If you want logic when creating an object, you'll have to use factory functions

## Examples

For the rest of this Introduction, I am going to show some example Py++ code, and then show what C++ code it transpiles into. Because in my opinion, if you already have some understanding of C++, then the best way to learn how to write Py++ code is to understand how Py++ code translates to C++ code. 

You will see that each statement/expression that you write in Py++ code translates 1-to-1 to a statement/expression in the C++ generated code.

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

- You can see in the C++ code that the first two function parameters are pass-by-reference (as I mentioned above, this is the default)
- You can see that the integer is pass-by-value however, this is because primitive types like int, float, bool, int16_t, etc. are allways pass-by-value in Py++
- You can see how each statement/expression in the Py++ code translates 1-to-1 to a statement/expression the C++ code (as mentioned above)
- You can see that the types 'list' and 'str' in the Py++ code translate to pypp::PyList and pypp::PyStr
    - PyList and PyStr are thin wrappers around std::vector and std::string respectively
    - Another note: Not shown in this example, but there is also pypp::PyDict and pypp::PySet (for 'dict' and 'set' types) which thinly wrap std::unordered_map and std::unordered_set, respectively
- You can see that the C++ code is wrapped in a 'me' namespace
    - All Py++ src files you write are translated to C++ files wrapped in a 'me' namespace

### 2) pass-by-value

As already mentioned above, if you surround a parameters type with 'Valu()', then the C++ code will be pass-by-value:

```python
# pass_by_value.py
from pypp_python import Valu

def pass_by_value_fn(a: Valu(list[int]), b: Valu(list[int])) -> list[int]:
    return a
```

this will transpile to C++ .h and .cpp files:

```cpp
// pass_by_value.h
#pragma once

#include "py_list.h"

namespace me {
pypp::PyList<int> pass_by_value_fn(pypp::PyList<int> a, pypp::PyList<int> b);
}
```

```cpp
// pass_by_value.h
#include "pass_by_value.h"

namespace me {
pypp::PyList<int> pass_by_value_fn(pypp::PyList<int> a, pypp::PyList<int> b) {
    return a;
}
}
```

- You can see in the C++ code, that the parameters are pass-by-value now (there is no '&' symbol)

### 3) return-by-reference

As already mentioned above, if you surround a return type with 'Ref()', then the C++ code will return-by-reference

```python
# return_by_reference.py
from pypp_python import Ref

def return_by_ref_function(a: list[int], b: list[int]) -> Ref(list[int]):
    return a
```

this will transpile to C++ .h and .cpp files:

```cpp
// return_by_reference.h
#pragma once

#include "py_list.h"

namespace me {
pypp::PyList<int> &return_by_ref_function(pypp::PyList<int> &a, pypp::PyList<int> &b);
}
```

```cpp
// return_by_reference.h
#include "return_by_reference.h"

namespace me {
pypp::PyList<int> &return_by_ref_function(pypp::PyList<int> &a, pypp::PyList<int> &b) {
    return a;
}
}
```

- You can see in the C++ code, that the return type is return-by-reference now.

### 4) Union and Optional types

If you are used to using Python's isinstance() function to check the type of an object, you can do something very similar in Py++.

If you add the following function to a Py++ src file

```python
# union_example.py
from pypp_python import Uni, ug, isinst, is_none


def union_example():
    int_float_or_list: Uni[int, float, list[int]] = Uni(3.14)
    if isinst(int_float_or_list, float):
        val: float = ug(int_float_or_list, float)
        print(val)
    # Union with None
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

- You can see that for union and optional type (which is just a union type with a None), we use the 'Uni' type imported from pypp_python
- You can see that methods isinst (i.e. isinstance), and is_none, also imported from pypp_python, are used
- You can see that 'ug' (i.e. union get), is used to get the actual value

### 5) classes

If you add the following class to a Py++ src file

```python
# greeter.py
from dataclasses import dataclass


@dataclass(frozen=True, slots=True)
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
    const pypp::PyStr &name;
    const pypp::PyStr &prefix;
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

- You can see that we use the @dataclass annotation
    - You can use frozen or slots options if you want
    - Small note: if you use the frozen option, then the generated C++ structure will use 'const' for the instance variables
- You can see that the instance variables 'name' and 'prefix' are stored as references
    - You can change this also to be stored as values by wrapping the type with 'Valu()' in the Py++ code
- You can see that we did not put any login in a constructor
    - As I mentioned above, if you want to create objects with some logic, you have to use factory functions
- You can see that f-strings are supported, by being translated to std::format

### 6) mov (std::move)

TODO