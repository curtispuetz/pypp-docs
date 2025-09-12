# Config classes

Config classes are used to group together constant variables.

## Declaration

```python
from pypp_python import configclass


@configclass
class MyConfig:
    a: int = 1
    b: str = "2"
```

If all the variables have the same type, you can also use the shorthand version with the `dtype` parameter.

```python
from pypp_python import configclass


@configclass(dtype=str)
class MyConfig2:
    a = "a"
    b = "b"
```

### How it translates to C++

For reference, the MyConfig class above translates to the following C++. It is one of the few statements that does not translate 1:1 to a C++ statement.

```cpp
#pragma once

#include "py_str.h"

namespace me {

struct __PseudoPyppNameMyConfig {
    int a = 1;
    pypp::PyStr b = pypp::PyStr("2");
};
inline __PseudoPyppNameMyConfig MyConfig;

} // namespace me
```

## Usage

Import your config class and use it in the normal Python way.

```python
from my_module import MyConfig


def pseudo_fn():
    print(MyConfig.a)
```
