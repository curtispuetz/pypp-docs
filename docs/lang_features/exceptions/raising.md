# Raising exceptions

Like Python `raise` statements, except the following is not supported:

- `from` (i.e. `raise ... from ...`)
- using a bare `raise` anywhere outside an except block

## Usage

```python
from my_module import some_process


def pseudo_fn(a: list[int]):
    # basic raise example
    if 42 not in a:
        raise ValueError("42 not found")

    # bare raise example
    try:
        some_process()
    except:
        print("an exception was thrown")
        raise
```