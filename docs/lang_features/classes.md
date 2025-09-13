# Classes

Like Python dataclasses, but magic methods are not supported today.

## Declaration

```python
from dataclasses import dataclass
from pypp_python import Valu


@dataclass
class Greeter:
    name: str
    prefix: Valu(str)

    def greet(self) -> str:
        return f"Hello, {self.prefix} {self.name}!"
```

- In Py++, the first argument of methods must be called `self`. Other names are not supported.
- The frozen and slots options (i.e. `@dataclass(frozen=True, slots=True)`) are supported
    - frozen makes the data members `const` in the generated C++
    - slots does not do anything to change the generated C++ code; it only affects the Python interpreter

## Instantiation

```python
from pypp_python import auto


def pseudo_fn():
    # without auto
    bob: str = "Bob"
    greeter1: Greeter = Greeter(bob, "Mr.")
    
    # with auto
    jane: str = "Jane"
    greeter2: auto = Greeter(jane, "Mrs.")
```

## Factory function pattern

In Py++ you don't define a constructor for the class. If you want instantiation of objects with some other logic, you can use a factory function pattern. An example:

```python
def greeter_mrs_factory(name: str, is_doctor: bool) -> Greeter:
    return Greeter(name, "Dr." if is_doctor else "Mrs.")
```