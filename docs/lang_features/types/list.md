# lists

Like Python lists.

## Declaration

### Basic
Similar to Python:

```python
def pseudo_fn():
    # empty
    empty_list: list[int] = []

    # empty option 2
    empty_list_2: list[int] = list[int]()

    # with some values
    short_list: list[int] = [1, 2, 3, 4]

    # repeating
    repeating_list: list[int] = [1, 2, 3] * 50

    # from a different data structure (set, dict, or str)
    list_of_strs: list[str] = list("hello")

    # copying another list
    list_copy: list[int] = short_list.copy()

    # copying another list option 2 and 3
    list_copy_2: list[int] = list(short_list)
    list_copy_3: list[int] = short_list[:]
```

### From size and default value
In C++, you can initialize a `std::vector` efficiently by just passing it a size and, optionally, a default value (i.e. `std::vector<int> my_vec(1000, 2)`). In Py++, you can get this efficiency also with the `int_list`, `float_list`, and `str_list` functions:

```python
from pypp_python import int_list, float_list, str_list

def pseudo_fn():
    # Without defaults
    a: list[int] = int_list(1000)
    b: list[float] = float_list(1000)
    c: list[str] = str_list(1000)

    # With defaults
    d: list[int] = int_list(1000, 2)
    e: list[float] = float_list(1000, 1.1)
    f: list[str] = str_list(1000, "hello")
```

For reference, this translates to the following .h and .cpp files:

```cpp
#pragma once

namespace me {
void pseudo_fn();
} // namespace me
```

```cpp
#include "lists/declaration.h"
#include "py_list.h"
#include "py_str.h"
#include "pypp_util/print.h"

namespace me {
void pseudo_fn() {
    pypp::PyList<int> a(1000);
    pypp::PyList<double> b(1000);
    pypp::PyList<pypp::PyStr> c(1000);
    pypp::PyList<int> d(1000, 99);
    pypp::PyList<double> e(1000, 1.1);
    pypp::PyList<pypp::PyStr> f(1000, pypp::PyStr("hello"));
}

} // namespace me
```

### List Comprehensions

Are supported in Py++. See [comprehensions](../comprehensions.md).


## Assigning an element

Same as Python
```python
def pseudo_fn(my_list: list[str]):
    my_list[5] = "hello"
```

## Accessing an element

Using the `[]` operator is supported, but it will give undefined behavior if the index is out of range, and it does not support negative indices like Python.

If you want negative indices support and error handling similar to Python, where an `IndexError` is thrown when the index is out of range, you can use the `lg` built-in function


```python
from pypp_python import lg


def pseudo_fn(my_list: list[str]):
    a: str = my_list[5]  # undefined behavior if 5 is out of range, but is really fast

    b: str = lg(my_list, 5)  # will throw `IndexError` if 5 is out of range

    c: str = lg(my_list, -2)  # supports negative indices
```

`lg` stands for 'list-get'

## Accessing multiple elements

Same as Python with slices

```python
def pseudo_fn(my_list: list[str]):
    a: list[str] = my_list[1:4]
    b: list[str] = my_list[1::2]
    c: list[str] = my_list[::2]
    d: list[str] = my_list[5:1:-1]
    # etc.

    # Or, with a slice object
    my_slice: slice = slice(0, 100, 2)
    e: list[str] = my_list[my_slice]
```

## Deleting elements

Use the pop method

## Methods

All of the Python list methods are supported in Py++.

The methods are listed below and documented in the Python docs.
- `append`
- `extend`
- `insert`
- `remove`
- `pop`
- `clear`
- `index`
- `count`
- `sort`
- `reverse`
- `copy`

Note: you can only pass temporaries or use `mov()` with the `append`, `extend`, and `insert` methods.

## list_reserve

For a list, you can call the list_reserve function like this:

```python
from pypp_python import list_reserve

def pseudo_fn():
    a: list[int] = []
    list_reserve(a, 100000)
```

What this does is call the .reserve() method on the std::vector in C++. This can improve efficiency a lot when creating large lists.

Note: if you are running Py++ with the Python interpreter, list_reserve does nothing.

## Other operations

Supported with the same behavior as Python:

- `in`, `not in`
- `+`, `+=` (concatentation)
- `*`, `*=` (repitition)
- `==`, `!=`
- `<`, `>`, `<=`, `>=`
- `len()`
- `min()`, `max()`
- `set()` 
- `dict()` (if the list is of len(2) tuples)
- `print()`

## Iteration support

Same as Python

```python
def pseudo_fn(colors: list[str]):
    for color in colors:
        print(color)
```
