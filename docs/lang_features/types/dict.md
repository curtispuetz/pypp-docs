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

Are supported in Py++. See [comprehensions](../comprehensions.md).

## Assigning a key-value pair

Same as Python

```python
def pseudo_fn(my_dict: dict[int, str]):
    my_dict[2] = "hello, dict!"
```

## Accessing a value

You can use the `[]` operator if you are OK with undefined behavior if the key does not exist. If you want an error handling experience similar to Python where a `KeyError` is thrown when the key does not exist, you can use the `dg` built-in unction:

```python
from pypp_python import dg

def pseudo_fn(d: dict[int, str]):
    a: str = dict[5]  # undefined behavior if key does not exist

    b: str = dg(d, 5)  # will throw `KeyError` if key does not exist
```

`dg` stands for 'dict-get'

## Deleting key-value pairs

Use the pop method

## Methods

Most of the Python dict methods are supported in Py++, and for some of them, there are some slight differences compared to the Python methods.

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

The following sections explain the slight differences compared to Python.

Note: you can only pass temporaries or use `mov()` with the `update` and `setdefault` methods.

### get

The get method must be used with a default value (in Python, it is optional).

```python
def pseudo_fn(d: dict[int, str]):
    value: str = d.get(10, "default value")
```

### setdefault

The setdefault method must be used with a default value (in Python, it is optional).

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

They are meant to be used in for loops only. I.e.:

```python
def pseudo_fn(d: dict[int, str]):
    for k in d.keys():
        print(k)

    for v in d.values():
        print(v)

    for k, v in d.items():
        print(k, v)
```
#### Items

`items()` can only be used exactly as shown in the above example. It must have two targets instead of one and it cannot be used with `enumerate()` or `zip()`. 

```python
def pseudo_fn(d: dict[int, str]):
    # ❌ can't do this
    for k_v_pair in d.items():
        print(k_v_pair)
    
    # ❌ can't do this
    for i, k_v_pair in enumerate(d.items()):
        print(k_v_pair)
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

See [above](#keys-values-and-items)
