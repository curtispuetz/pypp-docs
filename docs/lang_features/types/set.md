# Sets

Like Python sets.

## Declaration

### Basic
Similar to Python:

```python
from pypp_python import auto


def pseudo_fn():
    # empty
    empty_set: auto = set[int]()

    # with some values
    short_set: set[int] = {1, 2, 3, 4}

    # from a different datastructure (list, dict, or str)
    set_of_ints: set[int] = set(list_of_ints)

    # copying another set
    set_copy: set[int] = short_set.copy()

    # copying another set option 2
    set_copy_2: set[int] = set(short_set)
```

### Set Comprehensions

Are supported in Py++. See [comprehensions](comprehensions.md).

## Deleting elements

Use the remove or discard method

## Methods

All of the Python set methods are supported in Py++.

The methods are listed below and documented in the Python docs.
- `add`
- `remove`
- `discard`
- `pop`
- `clear`
- `isdisjoint`
- `issubset`
- `issuperset`
- `union`
- `intersection`
- `difference`
- `symmetric_difference`
- `update`
- `difference_update`
- `symmetric_difference_update`
- `copy`

Note: you can only pass temporaries or use `mov()` with the `add` method.


## Other operations

Supported with the same behavior as Python:

- `in`, `not in`
- `==`, `!=`
- `len()`
- `min()`, `max()`
- `list()`
- `print()`

Note: Other set operators like >, |, &, and ^, are not supported yet. For now, their corresponding methods can be used.

## Iteration support

Same as Python

```python
def pseudo_fn(colors: set[str]):
    for color in colors:
        print(color)
```