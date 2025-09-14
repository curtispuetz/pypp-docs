# Math

Like Python's math standard library. Limited at the moment.

## Usage

```python
import math


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

Note: others may also work. If the Python math function name (i.e. `math.my_name`) corresponds exactly to the C++ std:: `<cmath>` function name (i.e. `std::my_name`), then it will likely work, because the Py++ transpiler just translates `math.my_name` to `std::my_name`. I will do testing and support the other math functions later.

## Supported attributes

- `pi`

