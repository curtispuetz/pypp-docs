# Functions

Like Python functions, but keyword arguments, variable number of arguments, defaults, and type parameters (i.e. templates) are not supported for now.

The defaults and type parameters features will be coming soon.

## Declaration

This was mentioned elsewhere, but mentioning it again:
- Function parameters that are [primitive types](types/primitive_types.md) are always pass-by-value. 
- Function parameters that are non-primitive types are pass-by-reference by default, and can be made pass-by-value with `Valu()`.
- Functions are return-by-value by default, and can be made return-by-reference with `Ref()` for non-primtive types

```python
from pypp_python import int_list, mov
from my_type import MyType


# with primitive types (pass-by-value)
def add(a: int, b: int) -> int:
    return a + b


# pass-by-reference
def repeat_new(a: list[int]) -> list[int]:
    return a * 2


# pass-by-value
def my_type_factory(a: Valu(list[int])) -> MyType:
    return MyType(mov(a))


# return-by-reference
def repeat(a: list[int]) -> Ref(list[int]):
    a *= 2
    return a

```

## Invoking

```python
from pypp_python import auto, Ref, mov
from my_module import add, repeat_new, my_type_factory, repeat
from my_type import MyType


def pseudo_fn():
    add(1, 2)

    a: list[int] = [1, 2, 3]
    a_repeated: list[int] = repeat_new(a)

    my_type_0: MyType = my_type_factory([9, 8, 7])
    b: list[int] = [1, 2, 3]
    my_type_1: MyType = my_type_factory(mov(b))

    c: list[int] = [1, 2]
    c_ref: Ref(list[int]) = repeat(c)
```

## Void functions

If you want to define a function which does not return anything, you do not specify any return type. In these types of function, you can use an empty `return` statement if you want to exit early.

```python
def a_void_fn():
    for i in range(3):
        if i == 1:
            return
```

## Assigning a function to a variable

Like in Python

```python
from typing import Callable


def sum_fn(a: int, b: int) -> int:
    return a + b


def uses_function(fn: Callable[[int, int], int]) -> int:
    return fn(1, 2)
    

def pseudo_fn():
    fn_var: Callable[[int, int], str] = sum_fn

    uses_function(fn_var)

```