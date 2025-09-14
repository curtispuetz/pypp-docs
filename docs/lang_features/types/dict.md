# Dictionaries

Like Python dictionaries, but Py++ dictionaries are unordered.

## Declaration

### Basic
Similar to Python:

```python
def pseudo_fn():
    # empty
    empty_dict: dict[int, str] = {}

    # empty option 2
    empty_dict_2: dict[int, str] = dict[int, str]()

    # with some values
    short_dict: dict[int, str] = {
        0: "zero",
        1: "one"
    }

    # from a list of len(2) tuples
    var: dict[int, str] = dict([(0, "zero"), (1: "one")])

    # copying another dict
    dict_copy: dict[int, str] = short_dict.copy()

    # copying another dict option 2
    dict_copy_2: dict[int, str] = dict(short_dict)

```

### Dictionary Comprehensions

Are supported in Py++. See [comprehensions](comprehensions.md).

## Assigning a key-value pair

Same as Python

```python
def pseudo_fn(my_dict: dict[int, str]):
    my_dict[2] = "hello, dict!"
```

## Accessing a value

Unfortunately, it is not recommended to use the `[]` operator to access a value. Instead, it is recommended to use the `dg` function:

```python
from pypp_python import dg

def pseudo_fn(d: dict[int, str]):
    index: int = 5
    value: str = dg(d, index)
```

`dg` stands for 'dict-get'

### Note about using `[]` operator

If the key that you are using exists in the dictionary, then using the '[]' operator will work as expected. But, if the key does not exist, then no runtime `KeyError` error will be thrown (like how Python throws a `KeyError` in this case), and undefined behavior will occur. This is why its not recommended to use '[]', whereas `dg` does throw the `KeyError`, just like Python.

## Deleting key-value pairs

Use the pop method

## Methods

Most of the Python dict methods are supported in Py++, and for some of them there are some slight differences compared to the Python methods.

These are the supported methods

- `get`
- `pop`
- `update`
- `setdefault`
- `keys`
- `values`
- `items`
- `clear`
- `copy`

The below sections explain the slight differences compared to Python.

Note: you can only pass temporaries or use `mov()` with the `update`, and `setdefault` methods.


### get

The get method must be used with a default value (in Python it is optional).

```python
def pseudo_fn(d: dict[int, str]):
    value: str = d.get(10, "default value")
```

### setdefault

The setdefault method must be used with a default value (in Python it is optional).

```python
def pseudo_fn(d: dict[int, str]):
    d.setdefault(10, "default value")

```

### keys, values, and items

keys, values, and items work the same as in Python, but you cannot pass them as arguments. I.e. you cannot do something this:

```python
from collections.abc import KeysView

# ❌ can't do this
def pseudo_fn(keys: KeysView[int]):
    pass
```

They are meant to be used for iterations only. I.e.:

```python
def pseudo_fn(d: dict[int, str]):
    for k in d.keys():
        print(k)

    for v in d.values():
        print(v)

    for k, v in d.items():
        print(k, v)
```

## Other operations

Supported with the same behavior as Python:

- `in`, `not in`
- `==`, `!=`
- `len()`
- `min()`, `max()`
- `list()`
- `set()`
- `print()`


## Other notes about usage

There are some places where you cannot inline dictionaries. For example:

```python
def pseudo_fn(d: dict[int, str]):
    # ❌ can't inline dict here
    if d == {0: "zero", 1: "one"}:
        print("equal")
```

You can inline dictionaries as function arguments.

## Iteration support

Same as Python, with the `items`, `keys`, and `values` methods.