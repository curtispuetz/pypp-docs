# Interfaces

Like Python interfaces with the abc module, but you can only specify required methods (`properties` are not supported).

## Declaration

```python
from pypp_python import ABC, abstractmethod

class MyInterface(ABC):
    @abstractmethod
    def do_something(self):
        pass

    @abstractmethod
    def another_method(self, arg: int):
        pass
```

Note: whatever you put inside the `abstractmethod` bodies is ignored, so it is best to put `pass`.

## Usage

Must implement all methods from the interface

```python
from pypp_python import dataclass


@dataclass
class Implementation1(MyInterface):
    my_list: list[int]

    def do_something(self):
        my_list.append(42)
    
    def another_method(self, arg: int):
        my_list.append(arg * 10)
```
