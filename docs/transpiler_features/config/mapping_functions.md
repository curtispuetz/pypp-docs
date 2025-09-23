# Transpiler config mapping functions

The examples for `name_map`, `call_map`, and `attr_map` on the [examples page](examples.md) showed usage of the `to_string` option. There are two other common options you can use: `custom_mapping` and `custom_mapping_starts_with`. When you use one of these options, you provide
a Python file with a Python function, which is code that you write that returns the string that the Py++ transpiler should translate the statement/expression into.

I think sometimes it will be necessary to use these for integrating certain C++ libraries. Other times, they can make your transpiler config files shorter because you can use one mapping function instead of many `to_string` items.

[api link](API.md/#custom_mapping_object)

## Example

For example, if we have a `call_map.json` file:

```json
{
    "custom_mapping_starts_with": {
        "glfw.": {
            "mapping_function": "glfw_mapping.py",
            "py_required_import": {
                "module": "pypp_glfw",
                "name": "glfw"
            },
            "angle_includes": [
                "GLFW/glfw3.h"
            ]
        }
    }
}
```

```python
# glfw_mapping.py
def mapping_fn(node: ast.Call, d: Deps, caller_str: str) -> str:
    s: list[str] = caller_str.split(".")
    assert len(s) == 2, f"more than one dot found in {caller_str}"
    fn_camel_case: str = "".join(x.capitalize() for x in s[1].split("_") if x)
    return f"glfw{fn_camel_case}({d.handle_exprs(node.args)})"
```

then this will, for each call in your code that begins with `glfw.`, and when `glfw` is imported from `pypp_glfw`, replace the snake case function with camel case (i.e. it converts `glfw.poll_events()` to `glfwPollEvents()`). It will also add the `#include <GLFW/glfw3.h>` include.

I used something like this when I created a Py++ library for GLFW, because the API that I want is like this `glfw.fn_name()`, but the existing C++ GLFW library (i.e. `"GLFW/glfw3.h"`) is like this `glfwFnName()`.

## Usage notes

- The location you put these files is in `.pypp/transpiler_config/mapping_functions`
- Each mapping function file must have one function named `mapping_fn`, as that is the entry point
    - You can put other functions/logic to use in your file to modularize your code, but you cannot import anything you write in other files
- The mapping function must have the [correct parameters](#input-parameters), in terms of the number of parameters and type.
- You do not need to import anything inside the file
    - The only thing you could import is `ast`, and it doesn't matter if you do that or not
- The [`Deps` parameter](#the-deps-parameter) has some methods that you can use.


## Input parameters

### name_map.json

#### custom_mapping

```python
def mapping_fn(node: ast.Name, d: Deps):
```

#### custom_mapping_starts_with

```python
def mapping_fn(node: ast.Name, d: Deps, full_name: str):
```

### call_map.json

#### custom_mapping

```python
def mapping_fn(node: ast.Call, d: Deps):
```

#### custom_mapping_starts_with

```python
def mapping_fn(node: ast.Call, d: Deps, full_call: str):
```

### attr_map.json

#### custom_mapping

```python
def mapping_fn(node: ast.Attribute, d: Deps):
```

#### custom_mapping_starts_with

```python
def mapping_fn(node: ast.Attribute, d: Deps, full_attr: str):
```

### ann_assign_map.json

#### custom_mapping

```python
def mapping_fn(cpp_type: str, target: str, value: str, value_inside_parens: str):
```


#### custom_mapping_starts_with

```python
def mapping_fn(cpp_type: str, target: str, value: str, value_inside_parens: str):
```

## The `Deps` parameter

This parameter has some methods on it that you can use. In the above example, I showed the usage of the `handle_exprs` method. There is a good chance you will need the `handle_exprs` method also for whatever you are doing. In this section, I will document the methods. Only `handle_expr`, `handle_exprs`, `add_inc`, `value_err`, and `value_err_no_ast` are the ones I think will typically be useful. The others I don't see much use.

### handle_expr

Calculates the C++ code that a single expression should translate to

```python
def handle_expr(self, node: ast.expr) -> str
```

### handle_exprs

Calculates the C++ code that multiple expressions should translate to

```python
def handle_exprs(self, exprs: list[ast.expr], join_str: str = ", ") -> str
```

### handle_stmts

Calculates the C++ code that multiple statements should translate to

```python
def handle_stmts(self, stmts: list[ast.stmt]) -> str
```

### add_inc

Will add an include to the corresponding C++ file

```python
def add_inc(self, inc: CppInclude) -> None
```

Examples:

```python
d.add_inc(QInc("MyLib"))  # for quote includes
d.add_inc(AngInc("glad/gl.h"))  # for angle bracket includes
```

### add_incs

Will add includes to the corresponding C++ file

```python
def add_incs(self, incs: list[CppInclude]) -> None
```

### is_imported

Will check if something is imported for the current Python file.

```python
def is_imported(self, imp: PyImp) -> bool
```

Example:

```python
d.is_imported(PyImp("my_module", "my_name"))
```

### value_err

Will throw a value error with a message that includes the Python file from which the error originated and the problem code.

```python
def value_err(self, msg: str, ast_node) -> throws ValueError
```

Example:

```python
d.value_err("Wrong number of arguments found", call_node)
```

### value_err_no_ast

Will throw a value error with a message that includes the Python file from which the error originated.

```python
def value_err_no_ast(self, msg: str) -> throws ValueError
```
