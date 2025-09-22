# Imports

Like Python imports, except only `from ... import ...` statements are supported, and using `as` in these statements is not supported.

Relative imports are supported.

## Example

Say your Py++ project looks like this:

```text
pypp_project_dir/
    my_main_file.py
    my_source_file.py
    .pypp/
```

In your main file, you import from your source file in the usual way you would expect coming from Python

```python
# my_main_file.py
from my_source_file import hello_world

if __name__ == "__main__":
    hello_world()
```
