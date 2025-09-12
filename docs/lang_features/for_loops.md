# For loops

Like Python for loops, but you cannot modify the target unless using `range()`.

## Usage

Same as Python

```python
def pseudo_fn(integers: list[int], strings: list[str]):
    # typical
    for integer in integers:
        print(integer)

    # with range()
    for i in range(len(integers)):
        print(i)
    for i in range(len(integers), 5, -2):
        print(i)

    # with enumerate()
    for i, val in enumerate(integers):
        print(i, val)

    # with zip()
    for integer, string in zip(integers, strings):
        print(integer, string)

    # with reversed()
    for integer in reversed(integers):
        print(integer)
```

### How it translates to C++

For reference, the above Py++ code translates to the following C++ code:

```cpp
#pragma once

#include "py_list.h"
#include "py_str.h"

namespace me {
void pseudo_fn(pypp::PyList<int> &integers, pypp::PyList<pypp::PyStr> &strings);
} // namespace me
```

```cpp
#include "loops/for_examples.h"
#include "py_enumerate.h"
#include "py_reversed.h"
#include "py_zip.h"
#include "pypp_util/print.h"

namespace me {
void pseudo_fn(pypp::PyList<int> &integers,
               pypp::PyList<pypp::PyStr> &strings) {
    for (const auto &integer : integers) {
        pypp::print(integer);
    }
    for (int i = 0; i < integers.len(); i += 1) {
        pypp::print(i);
    }
    for (int i = integers.len(); i < 5; i += -2) {
        pypp::print(i);
    }
    for (const auto &[i, val] : pypp::PyEnumerate(integers)) {
        pypp::print(i, val);
    }
    for (const auto &[integer, string] : pypp::PyZip(integers, strings)) {
        pypp::print(integer, string);
    }
    for (const auto &integer : pypp::PyReversed(integers)) {
        pypp::print(integer);
    }
}

} // namespace me
```

## Enumerate, Zip, and Reversed

- Enumerate, zip, and reversed can only be used in for loops (i.e. you cannot assign the result of calling one of them to a variable)
- Enumerate, zip, and reversed can be about 2.5, 5, and 10 times slower respectivly than using `range`
    - In many practical cases there is no noticable performance difference
- The Python 'start' argument for enumerate is not supported

## Range

A range can be assigned to a variable and used as a parameter.

```python
def pseudo_fn():
    a: range = range(2, 10)
```

However, do not try to access the range attributes (start, stop, step), because they are not supported.

# Other notes about usage

Except for when using `range()`, the reason why you cannot modify the target of the for loop is because in the generated C++ the targets are const qualified

