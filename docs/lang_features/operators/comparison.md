# Comparison operators

Same as Python

## Supported

- `==`
- `!=`
- `<`
- `>`
- `<=`
- `>=`
- `in`
- `not in`
- `is`
- `not is`

Note: when you use `a is b` or `a is not b`, the Py++ transpiler just puts this as `&a == &b` and `&a != &b` in the generated C++ code.