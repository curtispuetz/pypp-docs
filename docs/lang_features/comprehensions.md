# Comprehensions

Like Python list, set, and dictionary comprehensions.


You can not inline comprehensions (i.e. you can only assign them to a variable), and multiple loops and ifs in the comprehensions are not supported.

Comprehensions are one of the few expressions in Py++ that doesn't map 1:1 to a expression in the generated C++ code (this is why you can't inline them). Below shows how to use comprehensions for lists, sets, and dictionaries, and their coorsponding generated C++ code.

## List

```python
from pypp_python import mov

def list_comp_fn():
    squares: list[int] = [x * x for x in range(10)]
    fibonacci: list[int] = [x + y for x, y in zip([0, 1], [1, 2])]
    sequence: list[int] = [mov(i) for i in range(10)]  # must use mov here
```

This translates to the following .h and .cpp files:


```cpp
#pragma once

namespace me {
void list_comp_fn();
} // namespace me
```

```cpp
#include "lists/comprehensions.h"
#include "py_list.h"
#include "py_zip.h"
#include <utility>

namespace me {
void list_comp_fn() {
    pypp::PyList<int> squares;
    for (int x = 0; x < 10; x += 1) {
        squares.append(x * x);
    }
    pypp::PyList<int> fibonacci;
    for (const auto &[x, y] :
         pypp::PyZip(pypp::PyList({0, 1}), pypp::PyList({1, 2}))) {
        fibonacci.append(x + y);
    }
    pypp::PyList<int> sequence;
    for (int i = 0; i < 10; i += 1) {
        sequence.append(std::move(i));
    }
}

} // namespace me
```

## Set

```python
from pypp_python import mov


def set_comp_fn():
    squares: set[int] = {x * x for x in range(4)}
    sequence: set[int] = {mov(i) for i in range(4)}  # must use mov here
```

This translates to the following .h and .cpp files:

```cpp
#pragma once

namespace me {
void set_comp_fn();
} // namespace me
```

```cpp
#include "sets/comprehensions.h"
#include "py_set.h"
#include <utility>

namespace me {
void set_comp_fn() {
    pypp::PySet<int> squares;
    for (int x = 0; x < 4; x += 1) {
        squares.add(x * x);
    }
    pypp::PySet<int> sequence;
    for (int i = 0; i < 4; i += 1) {
        sequence.add(std::move(i));
    }
}

} // namespace me
```

## Dictionary

```python
from pypp_python import mov


def dict_comp_fn():
    squares: dict[int, int] = {mov(x): x * x for x in range(4)}  # must use mov
```

This translates to the following .h and .cpp files:

```cpp
#pragma once

namespace me {
void dict_comp_fn();
} // namespace me
```

```cpp
#include "dicts/comprehensions.h"
#include "py_dict.h"
#include <utility>

namespace me {
void dict_comp_fn() {
    pypp::PyDict<int, int> squares;
    for (int x = 0; x < 4; x += 1) {
        squares[std::move(x)] = x * x;
    }
}

} // namespace me
```