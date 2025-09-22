# Random

Like Python's random standard library. Limited at the moment.

## Usage

```python
from pypp_python import random


def pseudo_fn():
    rng: random.Random = random.Random(42)
    rng.seed(123)
    my_list: list[int] = [1, 2, 3]
    print(
        f"float between [0.0, 1.0): {rng.random()}"
        f"int in [2, 10]: {rng.random(2, 10)}"
        f"random number in my_list: {rng.choice(my_list)}"
    )

    # shuffle the list
    rng.shuffle(my_list)
```

## Supported Random methods

- `seed`
- `random`
- `randint`
- `shuffle`
- `choice`

Note: shuffle only works with lists at the moment.
