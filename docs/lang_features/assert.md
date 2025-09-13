# Assertions

Exactly like Python

## Usage

```python
def pseudo_fn(a: list[int], b: list[int]):
    # without a message
    assert len(a) == len(b)

    # with a message
    assert max(a) == max(b), "lists should have equal max elements"
```

When running the C++ executable and an assertion triggers, it throws a `pypp::AssertionError` exception.