# Default Dictionaries

Like Python default dictionaries, but Py++ default dictionaries are unordered.

Only the things that are different from [dictionaries](dict.md) are mentioned on this page.

## Declaration
Similar to Python:

```python
from pypp_python import auto, defaultdict


def pseudo_fn():
    # empty
    a: auto = defaultdict[int, str](str)
    b: auto = defaultdict[int, list[str]](list[str])

    # empty with a custom default lambda
    c: auto = defaultdict[int, str](lambda: "default")
    d: auto = defaultdict[int, CustomType](lambda: CustomType())

    # with some initial values
    e: auto = defaultdict[int, str](str, {1: "one", 3: "three"})

    # copying another defaultdict
    f: auto = e.copy()
```

## Accessing a value

You can access a value with the '[]' operator, just like Python. We don't use the 'dg' function like we do for regular dictionaries because accessing a default dictionary never throws an exception when trying to access a value. Instead, for a default dictionary, if the key does not exist, the default value is added for that key-value pair, just like Python.

```python
my_default_dict[0]
```

## Other operations

Supported with the same behavior as Python:

- `dict()`

Also supported are all the ones mentioned in [dictionaries](dict.md#other-operations).

Note: for comparison operators (i.e. `==` and `!=`) between a dict and a defaultdict, the defaultdict must be on the left side (otherwise C++ will give a compilation error).
