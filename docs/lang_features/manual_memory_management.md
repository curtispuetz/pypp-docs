# Manual memory management

## Owner Vs. Reference

Py++ varaibles can either be an `owner` or a `reference to an owner`. If it is an owner, that means it is responsible for managing the lifetime of the data.


```python
from pypp_python import Ref, Valu, mov
from dataclasses import dataclass


@dataclass
class ClassA:
    # list_member_a is a reference to the owner
    list_member_a: list[int]


def class_a_factory(list_param_a: list[int]) -> ClassA:
    # list_param_a is a reference to the owner
    return ClassA(list_param_a)


@dataclass
class ClassB:
    # list_member_b is an owner
    list_member_b: Valu(list[int])


def class_b_factory(list_param_b: Valu(list[int])) -> ClassB:
    # list_param_b is an owner
    return ClassB(mov(list_param_b))


if __main__ == "__main__":
    my_list: list[int] = [1, 2, 3]  # my_list is an owner

    my_list_ref: Ref(list[int]) = my_list  # my_list_ref is a reference to the owner

    a: ClassA = class_a_factory(my_list)

    b: ClassB = class_b_factory([1, 2, 3])
```


## Pass-by-reference Vs. Pass-by-value

### For function parameters

Pass-by-reference is like in C++ when a function parameter has the `&` symbol, and pass-by-value is when it does not have any symbols.

The Py++ [primitive types](primitive_types.md) will always be pass-by-value (you cannot change that). Every other type is pass-by-reference by default, and can be made pass-by-value with `Valu()`.

```python
from pypp_python import Valu


def my_function(a: list[int], b: float, c: Valu(set[int])):
    # a is pass-by-reference
    # b is pass-by-value because it is a primitive type
    # c is pass-by-value because its type is wrapped in `Valu()`
    pass


if __main__ == "__main__":
    my_list: list[int] = [1, 2, 3]
    
    my_function(my_list, 3.14, {1, 2, 3})
```

### For class data members

For class data members it is the same as function parameters. Pass-by-reference is like in C++ when a data member is defined with the `&` symbol, and pass-by-value is when it does not have any symbols.

Like function parameters, the Py++ [primitive types](primitive_types.md) will always be pass-by-value, and every other type is pass-by-reference by default, and can be made pass-by-value with `Valu()`.

```python
from pypp_python import Valu
from dataclasses import dataclass


@dataclass
class ClassA:
    # a is pass-by-reference
    # b is pass-by-value because it is a primitive type
    # c is pass-by-value because its type is wrapped in `Valu()`
    a: list[int]
    b: float
    c: Valu(set[int])


if __main__ == "__main__":
    my_list: list[int] = [1, 2, 3]

    a: ClassA = ClassA(my_list, 3.14, {1, 2, 3})
```

## Return-by-value Vs. Return-by-reference

For functions and methods, return-by-value is like in C++ when the return type has no symbols, and return-by-reference is like when it has the `&` symbol.

[Primitive types](primitive_types.md) will always be return-by-value. Every other type is return-by-value by default, and can be made return-by-reference with `Ref()`.

```python
from pypp_python import Ref, Valu
from dataclasses import dataclass



# return-by-value
def create_list() -> list[int]:
    return [1, 2, 3]


@dataclass
class ClassA:
    list_member: Valu(list[int])

    # return-by-reference
    def get_list_member() -> Ref(list[int]):
        return list_member


if __main__ == "__main__":
    my_list: list[int] = create_list()

    a: ClassA = ClassA(my_list)

    my_list_ref = a.get_list_member()
```

## Using references to an owner

In Py++, if you have references to an owner, you are responsible for ensuring that you do not use any of these references after the lifetime of the owner has ended. This is the big rule in all programming languages with manual memory management.

## Using pass-by-value

When working with a function parameter or class data member that is pass-by-value, it is recommended that you only pass a temporary argument or pass using `mov()`. In fact, in a later Py++ version, it might become a transpilation error if you try to pass something else.

I'll show now what a temporary is and what `mov()` is.

### Temporaries

A temporary is an object created as a result of an expression but is not explicitly named.

```python
from pypp_python import Valu
from dataclasses import dataclass


@dataclass
class ClassB:
    list_member_b: Valu(list[int])


if __main__ == "__main__":
    b: ClassB = ClassB([1, 2, 3])  # [1, 2, 3] is a temporary
```

### mov()

`mov()` is like `std::move()` in C++. In fact, the Py++ transpiler just translates `mov()` to `std::move()`. It is a way of moving ownership of data to somewhere else.

Note: after calling `mov(v)`, you should not use `v` anymore (just like in C++).

```python
from pypp_python import Valu, mov
from dataclasses import dataclass


@dataclass
class ClassB:
    list_member_b: Valu(list[int])


if __main__ == "__main__":
    my_list: list[int] = [1, 2, 3]  # my_list is an owner.

    b: ClassB = ClassB(mov(my_list))  # after this, b.list_member_b owns the data.

    # my_list should not be used anymore
```