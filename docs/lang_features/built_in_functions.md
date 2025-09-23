# Built-in functions

Similar to Python built-in functions. Lots of the common Python built-in functions are supported.

Note: Any function which does not have to be imported or is imported from `pypp_python` is referred to as a "built-in function".

## Type conversion

### Data structures
- `list`
- `set`
- `dict`

### Numbers
- `int`
- `float`
- `to_float32`
- `to_int8_t`
- `to_int16_t`
- `to_int32_t`
- `to_int64_t`
- `to_uint8_t`
- `to_uint16_t`
- `to_uint32_t`
- `to_uint64_t`

`int()`, `float()`, and `to_float32()` can throw ValueError.

### Other

- `str`
- `bool`

### for strings

- `to_std_string`
- `to_c_string`

## Mathematical

- `max`
- `min`
- `int_pow` (** operator for integers)

`abs`, `sum`, and `round` are not supported. I'll likely add them soon.

## Input/Output

- `print`

## Utility

- `len`
- `open`

`open` can only be used with a (see [file I/O](file_io.md))

## Iterating and sorting

- `range`
- `enumerate`
- `reversed`
- `zip`

For usage of `range`, `enumerate`, `reversed`, and `zip`, see [for loop page](for_loops.md).

`sorted` is not supported.

## List initializers and reserve

- `int_list`
- `float_list`
- `str_list`
- `list_reserve`

## 'Get' functions

- `dg`
- `tg`
- `lg`
- `ug`

## Union functions

- `isinst`
- `is_none`

## Memory management tools

- `mov`

## Resources

- `res_dir`
