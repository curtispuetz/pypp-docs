# Math

Like Python's math standard library. Limited at the moment.

## Usage

```python
from pypp_python.stl import math


def pseudo_fn():
    sqrt_result: float = math.sqrt(9)
    hypot_result: float = math.hypot(3, 4)
    floor_result: int = math.floor(3.5)
    ceil_result: int = math.ceil(3.5)
    sin_result: float = math.sin(math.pi / 2)
```

## Supported functions

- `sqrt`
- `hypot`
- `floor`
- `ceil`
- `sin`
- `cos`
- `tan`
- `radians`

## Supported attributes

- `pi`


## Note about other functions and attributes

I will add support and testing for other math functions as time allows.

Others functions and attributes may also work. If the Python math function/attribute name (i.e. `math.my_name`) corresponds exactly to the C++ std:: `<cmath>` function/attribute name (i.e. `std::my_name`), then it will likely work, because the Py++ transpiler just translates `math.my_name` to `std::my_name`. 
