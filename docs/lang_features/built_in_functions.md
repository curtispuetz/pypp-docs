# Built-in functions

Similar to Python

## Type conversion

- `list`
- `set`
- `dict`
- `str`
- `bool`
- `int`
- `float`
- `to_float32`
- `to_int8_t`
- `to_int16_t`
- `to_int32_t`
- `to_int64_t`
- `to_uint8_t`
- `to_uint16_t`
- `to_uint32_t`
- `to_uint64_t`

### Usage

To use the later ones, they can be imported

```python
from pypp_python import (
    to_float32,
    to_int8_t,
    to_int16_t,
    to_int32_t,
    to_int64_t,
    to_uint8_t,
    to_uint16_t,
    to_uint32_t,
    to_uint64_t,
)
```

Note: even though we have to import them, we will still call them built-in functions.


## Mathematical

- `max`
- `min`

`abs`, `sum`, and `round` are not supported. I'll likely add them soon.

## Input/Output

- `print`

## Utility

- `len`

## Iterating and sorting

- `range`
- `enumerate`
- `reversed`
- `zip`

For usage of `range`, `enumerate`, `reversed`, and `zip`  see [for loop page](for_loops.md).

`sorted` is not supported.
