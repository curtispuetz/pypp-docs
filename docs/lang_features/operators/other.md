# Other operators

Like Python, except `@` is not supported.

## Supported

- `+`
- `-`
- `*`
- `/`
- `//`
- `**`
- `%`
- `<<`
- `>>`
- `|`
- `^`
- `&`

All of these are supported for augmented assignment too (i.e. `+=`, `-=`, `^=`, etc.), except for floor division (`//=`) and pow (`**=`) are not.

## Floor division and pow

- When you use `a ** b` and `a // b`, the Py++ transpiler translates them to `std::pow(a, b)` and `pypp::py_floor_div(a, b)` respectively.
