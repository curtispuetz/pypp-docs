# File IO

Like Python file I/O with `open()`.

## Usage

```python
from pypp_python import os, res_dir


def pseudo_fn():
    resources_dir: str = res_dir()
    text_file: str = os.path.join(resources_dir, "text.txt")
    with open(text_file, "w") as file:
        file.write("Line 1\n")
        file.write("Line 2\n")
```

`res_dir()` is a built-in function that gives us the path to the 'resources' project directory.

Note: `with` statements are only supported in Py++ for usage with `open()`.

## Supported `open()` modes

Like Python modes

- `"r"`
- `"w"`
- `"a"`
- `"r+"`
- `"w+"`
- `"a+"`

## Supported file I/O methods

Like Python methods

- `read`
- `readline`
- `readlines`
- `write`
- `writelines`

The methods can throw `RuntimeError`.