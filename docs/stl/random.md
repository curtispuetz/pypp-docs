# Random

Like Python's random standard library. Limited at the moment.

## Usage

```python
from pypp_python.stl import Random


def pseudo_fn():
    rng: Random = Random(42)

    # rng.seed
    rng.seed(123)
    my_list: list[int] = [1, 2, 3]

    # rng.random, rng.randint, rng.choice
    print(
        f"float between [0.0, 1.0): {rng.random()}"
        f"int in [2, 10]: {rng.randint(2, 10)}"
        f"random number in my_list: {rng.choice(my_list)}"
    )

    # rng.shuffle
    rng.shuffle(my_list)
```

## Supported Random methods

- `seed`
- `random`
- `randint`
- `shuffle`
- `choice`

Note: shuffle only works with lists at the moment.
