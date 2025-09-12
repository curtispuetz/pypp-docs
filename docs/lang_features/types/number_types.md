# Number types

## Basics
- `int`
- `float`

`float` is transalted to a `double` in the C++ code.

## Others

- `int8_t`
- `int32_t`
- `int64_t`
- `int16_t`
- `uint8_t`
- `uint16_t`
- `uint32_t`
- `uint64_t`
- `float32`


The integers translate to the types from the C `cstdint` library of the same names.

`float32` is translated to a `float` in the C++ code.

### How to use

```python
from pypp_python import (
    int8_t,
    int32_t,
    int64_t,
    int16_t,
    uint8_t,
    uint16_t,
    uint32_t,
    uint64_t,
    float32,
)
```

### Note about running with Python interpreter

If running with the Python interpreter, then all these types are just regular `int` and `float`.