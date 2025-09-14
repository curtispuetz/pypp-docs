# Solving undefined behavior

I mentioned in the [index page](../index.md) that Py++ would aspire to throw transpiler errors for programs that lead to undefined behavior and programs which run differently via the C++ executable vs. the Python interpreter.

I came up with a set of rules that need to be followed, where if you follow them, then your code will run the same via the C++ executable and Python interpreter. If we get to a point where the Py++ transpiler throws errors for each of these rules, when broken, then we will be in a very good place. Right now, none of these rules, when broken, throw transpiler errors.

If I am missing any rules, i.e. there are other ways you can get code that runs differently via C++ and Python, please let me know.

## Rules

- Only reassign a variable which is an owner and has no references
- For a function/method parameter that is pass-by-value, only pass temporaries or use `mov()`
- For a class data member that is pass-by-value, only pass temporaries or use `mov()` (similar to above)
- After doing `mov(v)`, do not use `v`
- Only use `mov(v)` if `v` is an owner and has no references
- Do not end the lifetime of an owner if the owner has any living references 
- In a return-by-value function/method, do not return a variable whose lifetime does not end at the end of the function/method
- When calling a return-by-reference function/method, the type annotation of the variable you assign the result to must be wrapped in `Ref()`
- When initializing list, set, dict, or tuple data structures with some initial values, only pass temporaries or use `mov()` for the elements


## Examples of each rule

### Only reassign a variable which is an owner and has no references

```python
from dataclasses import dataclass


@dataclass
class ClassA:
    my_list: list[int]


if __name__ == "__main__":
    my_list: list[int] = [1, 2, 3]
    my_list = [2, 3, 4]  # OK
    object_a: ClassA = ClassA(my_list)
    object_a.my_list = [4, 5, 6]  # X it is not the original
    my_list = [7, 8, 9]  # X it is the original, but it has a reference
```

### For a function/method parameter that is pass-by-value, only pass temporaries or use `mov()`

```python
from pypp_python import Valu, mov, auto


@dataclass
class ClassA:
    my_list: Valu(list[int])
    my_str: str


def a_factory(my_list: Valu(list[int])) -> ClassA:
    return ClassA(mov(my_list), "hello")


if __name__ == "__main__":
    object_a0: auto = a_factory([1, 2, 3])  # OK

    my_list_0: list[int] = [1, 2, 3]
    object_a1: auto = a_factory(mov(my_list_0))  # OK

    my_list_1: list[int] = [1, 2, 3]
    object_a2: auto = a_factory(my_list_1)  # X
```

### For a class data member that is pass-by-value, only pass temporaries or use `mov()` (similar to above)

```python
from pypp_python import Valu, mov, auto


@dataclass
class ClassA:
    my_list: Valu(list[int])


if __name__ == "__main__":
    object_a0: auto = ClassA([1, 2, 3])  # OK

    my_list_0: list[int] = [1, 2, 3]
    object_a1: auto = ClassA(mov(my_list_0))  # OK

    my_list_1: list[int] = [1, 2, 3]
    object_a2: auto = ClassA(my_list_1)  # X
```

### After doing `mov(v)`, do not use `v`

```python
from pypp_python import Valu, mov, auto


@dataclass
class ClassA:
    my_list: Valu(list[int])


if __name__ == "__main__":
    my_list_0: list[int] = [1, 2, 3]
    object_a1: auto = ClassA(mov(my_list_0))

    min_val: int = min(my_list_0)  # X
```

### Only use `mov(v)` if `v` is an owner and has no references

```python
from pypp_python import Valu, mov, auto


@dataclass
class ClassA:
    my_list: Valu(list[int])


def class_a_factory(my_list: Valu(list[int])) -> ClassA:
    return ClassA(mov(my_list))  # OK


@dataclass
class ClassB:
    my_list: list[int]

    def class_a_factory() -> ClassA:
        return ClassA(mov(self.my_list))  # X self.my_list is a reference


if __name__ == "__main__":
    my_list_0: list[int] = [1, 2, 3]
    object_a1: auto = class_a_factory(mov(my_list_0))  # OK

    my_list_1: list[int] = [1, 2, 3]
    my_list_ref: Ref(list[int]) = my_list_1

    object_a0: auto = class_a_factory(mov(my_list_ref))  # X my_list_ref is a reference

    my_list_2: list[int] = [1, 2, 3]
    my_list_2_ref: Ref(list[int]) = my_list_2
    object_a1: auto = class_a_factory(mov(my_list_2))  # X my_list_2 is the original, but it has a reference
```

### Do not end the lifetime of an owner if the owner has any living references 

```python
from pypp_python import Valu, mov, auto


@dataclass
class ClassA:
    my_list: list[int]


def a_factory() -> ClassA:
    my_list: list[int] = [1, 2, 3]
    object_a: auto = ClassA(my_list)  # X my_list lifetime ends when this function returns
    return object_a


if __name__ == "__main__":
    object_a: auto = a_factory()
```

### In a return-by-value function/method, do not return a variable whose lifetime does not end at the end of the function/method

```python
from pypp_python import auto, Valu
from dataclasses import dataclass


@dataclass
class ClassA:
    my_list: Valu(list[int])

    def get_my_list(self) -> list[int]:
        return self.my_list  # X my_list lifetime does not end


if __name__ == "__main__":
    object_a: auto = ClassA([1, 2, 3])
    my_list: list[int] = object_a.get_my_list()
```

### When calling a return-by-reference function/method, the type annotation of the variable you assign the result to must be wrapped in `Ref()`


```python
from pypp_python import auto, Valu, Ref
from dataclasses import dataclass


@dataclass
class ClassA:
    my_list: Valu(list[int])

    def get_my_list(self) -> Ref(list[int]):
        return self.my_list


if __name__ == "__main__":
    object_a: auto = ClassA([1, 2, 3])
    my_list_1: Ref(list[int]) = object_a.get_my_list()  # OK
    my_list_2: list[int] = object_a.get_my_list()  # X
```

### When initializing list, set, dict, or tuple data structures with some initial values, only pass temporaries or use `mov()` for the elements

TODO
