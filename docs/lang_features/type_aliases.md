# Type Aliases

Like Python type aliases, but must be defined using the `type` keyword (introduced in Python v3.12).

## Declaration

```python
type Matrix = list[list[int]]
type MyItems = tuple[str, float, int, list[str], dict[int, str]]
```

For reference, this translates to C++:

```cpp
#pragma once

#include "py_list.h"
#include "py_tuple.h"
#include "py_str.h"
#include "py_dict.h"

namespace me {
using Matrix = pypp::PyList<pypp::PyList<int>>;
using MyItems = pypp::PyTup<pypp::PyStr, double, int, pypp::PyList<pypp::PyStr>, pypp::PyDict<int, pypp::PyStr>>;
} // namespace me
```