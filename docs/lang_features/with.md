# With

Like Python with statements, but only usage with `open()` is supported.

## Usage

```python
import os
from pypp_python import res_dir


def pseudo_fn():
    resources_dir: str = res_dir()
    text_file: str = os.path.join(resources_dir, "text.txt")
    with open(text_file, "w") as file:
        file.write("Line 1\n")
        file.write("Line 2\n")
```

`res_dir()` is a built-in function that gives us the path to the 'resources' project directory.

## Supported `open()` modes

- `"r"`
- `"w"`
- `"a"`
- `"r+"`
- `"w+"`
- `"a+"`