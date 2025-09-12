# Imports

## Importing in main files

Say your Py++ project looks like this:

```text
pypp_project_dir/
    python/
        my_main_file.py
        src/
            my_src_file.py
```

In your main file, you import things from the src/ directory like this:

```python
# my_main_file.py
from my_src_file import hello_world

if __name__ == "__main__":
    hello_world()
```

Do not include `src.` in the import statement.

Only import from statements (i.e. `from ... import ...`) are supported.

## Importing in src files

You can only use import from statements (i.e. `from ... import ...`) in your src files to import things from other src files.

Relative imports do work.

## Additional restrictions
- You cannot use `as` in your import from statements
- Importing from other libraries, including the `pypp_python` library, should be done as shown in examples through this documentation. If you import things in a different way, the Py++ transpiler doesn't recognize what it is.
