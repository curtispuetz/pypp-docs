# Classes

Like Python dataclasses, but magic methods are not supported today.

## Declaration

```python
from dataclasses import dataclass
from pypp_python import Valu


@dataclass(frozen=True, slots=True)
class Greeter:
    name: str
    prefix: Valu(str)

    def greet(self) -> str:
        return f"Hello, {self.prefix} {self.name}!"
```

The frozen and slots options are supported (slots does not do anything to change the generated C++ code; its only for Python).

## Instantiation

```python
from pypp_python import auto


def pseudo_fn():
    bob: str = "Bob"
    greeter1: Greeter = Greeter(bob, "Mr.")
    # with auto
    jane: str = "Jane"
    greeter2: auto = Greeter(jane, "Mrs.")
```

Unrelated to classes, this is a teaching moment for Py++:

'name' is a pass-by-reference parameter and 'prefix' is a pass-by-value parameter. For pass-by-reference parameters, we cannot call with a temporary value. This is why we only used a temporary value for 'prefix' and not for 'name'.

## Factory function pattern

In Py++ you don't define a constructor for the class. If you want instantiation of objects with some other logic, you can use a factory function pattern. An example:

```python
def greeter_mrs_factory(name: str, is_doctor: bool) -> Greeter:
    return Greeter(name, "Dr." if is_doctor else "Mrs.")
```