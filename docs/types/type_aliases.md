# Type Aliases

Like Python type aliases, but must be defined using the `type` keyword (introduced in Python v3.12).

## Declaration

```python
type Matrix = list[list[int]]
type MyItems = tuple[str, float, int, list[str], dict[int, str]]
```

Note: These simply transpile to C++ with the C++ 'using' keyword.