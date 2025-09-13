# Custom exceptions

A custom exception type can be declared similarly to how it is done in Python, but you need an extra annotation.

## Declaration

```python
from pypp_python import exception


@exception
class MyCustomException(Exception):
    pass


# child exceptions
@exception
class MyChildException(MyCustomException):
    pass
```

## Usage

You can raise the exception like usual with a message

```python
def pseudo_fn():
    raise MyCustomException("something happened!")
```