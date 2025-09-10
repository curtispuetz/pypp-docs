# For loops

Like Python for loops, but you cannot modify the target.

## Usage

Same as Python

```python
def pseudo_fn(integers: list[int], strings: list[str]):
    # typical
    for integer in integers:
        print(integer)
    # with range()
    for i in range(len(integers)):
        print(i)
    for i in range(len(integers), 5, -2):
        print(i)
    # with enumerate()
    for i, val in enumerate(integers):
        print(i, val)
    # with zip()
    for integer, string in zip(integers, strings):
        print(integer, string)
    # with reversed()
    for integer in reversed(integers):
        print(integer)

```

## Enumerate, Zip, and Reversed

Enumerate, zip, and reversed can only be used in for loops like showed above.

## Range

Range can be assigned to a variable and used as a parameter.

```python
def pseudo_fn():
    a: range = range(2, 10)
```

However, do not try to access the range attributes (start, stop, step), because they are not supported.