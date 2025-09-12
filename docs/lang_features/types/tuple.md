# Tuple

Similar to Python tuples, but you have to individually define the type for each element and cannot iterate over them.

These are meant to be containers for multiple values, useful for returning multiple values from a function/method.

## Declaration

Same as Python

```python
def pseudo_fn():
    a: tuple[int, float, str] = (1, 1.2, "a")
```

## Accessing an element

Unfortunately, you cannot use the `[]` operator to access an element. Instead, you use the `tg` function:

```python
from pypp_python import tg

def pseudo_fn(t: tuple[int, float, str]):
    index: int = 1
    value: float = tg(d, index)
```

tg standard for 'tuple-get'

## Unpacking elements

Same as Python

```python
def pseudo_fn(t: tuple[int, float, str]):
    # from a variable
    a, b, c = t
    # from a function call
    u, v = create_len_2_tuple_fn()

```

## Methods

The two Python tuple methods are supported in Py++ and documented in the Python docs

- `count`
- `index`

## Other operations

- `in`, `not in`
- `==`, `!=`
- `<`, `>`, `<=`, `>=`
- `len()`
- `print()`