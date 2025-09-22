# Lambda functions

Like Python

In Py++, functions cannot be defined anywhere but the module level. However, lambda functions can be defined anywhere a variable can be defined.

## Declaration

```python
from pypp_python import Callable


def pseudo_fn():
    my_lambda: Callable[[int, int], str] = lambda x, y: f"result: {x, y}"
```

## Usage

```python
from pypp_python import Callable


def use_lambda(fn: Callable[[int, int], str]):
    print(fn(1, 2))


def pseudo_fn():
    use_lambda(lambda x, y: f"result: {x, y}")
```