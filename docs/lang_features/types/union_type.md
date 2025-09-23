# Unions

A small departure from Python, Py++ Unions are the way that Py++ handles variables which can be different types or which can be 'None'.

## Declaration

```python
from pypp_python import Uni, auto


def pseudo_fn():
    # without auto
    a: Uni[int, float, str, list[int]] = Uni("value")
    b: Uni[int, list[int]] = Uni([1, 2, 3])
    
    # with auto
    c: auto = Uni[float, str](3.14)
    
    # with a None option (i.e Optional)
    d: auto = Uni[str, None](None)
```

## Accessing the value

First, you should check the value type with the `isinst` function, and then get the value with the `ug` function.

```python
from pypp_python import Uni, ug, isinst, is_none


def int_float_union(u: Uni[int, float]):
    if isinst(u, float):
        val: float = ug(u, float)
```

For a Union with a possible 'None' type, there is an `is_none` function

```python
def optional_str_union(u: Uni[str, None]):
    if not is_none(u):
        string: str = ug(u, str)
```

`ug` stands for 'union-get'.

## Other operations

- `==`, `!=`
- `print()`

Note: The comparison operators are only supported if the unions have the same types and each of the types supports comparison operators.
