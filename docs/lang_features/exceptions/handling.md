# Exception handling

Like Python exception handling with `try...except` statements, but the following is not supported:

- `finally`
- `else`
- catching multiple exception types at once (i.e. `except (KeyError, IndexError) as e:`)
- doing something with the caught exception, `e`, other than `str(e)`

## Usage

```python
from my_module import (
    fn_that_throws_type_error, 
    fn_that_throws_index_or_type_error
)


def pseudo_fn():
    # basic
    try:
        fn_that_throws_type_error()
    except TypeError as e:
        print(f"Caught a TypeError: {str(e)}")

    # catch all exception types
    try:
        fn_that_throws_type_error()
    except:
        print(f"Caught an error.")
    
    # catching multiple different types
    try:
        fn_that_throws_index_or_type_error()
    except TypeError as e:
        print(f"Caught a TypeError: {str(e)}")
    except IndexError as e:
        print(f"Caught an IndexError: {str(e)}")
```

Note: Again, you can't do anything with `e` other than `str(e)`.
