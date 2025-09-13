# With

Like Python with statements, but only usage with `open()` is supported.

## Usage

```python
import os
from pypp_python import pypp_get_resoruces


def pseudo_fn():
    test_dir: str = pypp_get_resources("test_dir")
    text_file: str = os.path.join(test_dir, "text.txt")
    with open(text_file, "w") as file:
        file.write("Line 1\n")
        file.write("Line 2\n")
```

## Supported `open()` modes

- `"r"`
- `"w"`
- `"a"`
- `"r+"`
- `"w+"`
- `"a+"`