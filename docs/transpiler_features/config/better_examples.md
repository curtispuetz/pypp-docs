# Transpiler config better examples


## Using `required_py_import`

The [`name_map` example](examples.md#name_mapjson) is not great in practice because it will treat any `deque` in your code (for example, a function you've defined with the name `deque`), the same. To avoid this, you can specify a required import that must be in the file for the rule to apply. 

```json
{
    "to_string": {
        "deque": {
            "to": "MyLibraryDequeImplementation",
            "required_py_import": {
                "module": "my_deque_library",
                "name": "deque"
            }
        }
    }
}
```

This will make it so that `deque` is only translated to `MyLibraryDequeImplementation` if that import is in the file.

Note: you can also specify `required_py_import` in `call_map`, `attr_map`, `ann_assign_map`, `always_pass_by_value`, and `subscriptable_types`.

[api link](API.md/#required_py_import_object)

## Using `quote_includes` and `angle_includes`

For the `name_map`, `call_map`, `attr_map`, and  `ann_assign_map`, you can also specify `quote_includes` and `angle_includes`. This tells the Py++ transpiler to also add the specified include statements at the top of the generated C++ code, whenever the rule is applied. 

For example, if we continue with the same `deque` example, we could do:

```json
{
    "to_string": {
        "deque": {
            "to": "MyLibraryDequeImplementation",
            "required_py_import": {
                "module": "my_deque_library",
                "name": "deque"
            },
            "quote_includes": [
                "MyLibrary.h"
            ]
        }
    }
}
```

This tells the Py++ transpiler that when the rule is applied, to add an include `#include "MyLibrary.h"` in the generated C++ code. 

## Using custom mapping functions

Take a look at [this page](mapping_functions.md)
