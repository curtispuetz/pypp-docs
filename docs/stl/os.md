# Os

Like Python's os standard library. Limited at the moment.

## Usage

```python
from pypp_python import tg, res_dir
from pypp_python.stl import os


def pseudo_fn():
    resources: str = res_dir()
    # os.path.join
    new_file: str = os.path.join(resources, "new_file.txt")

    # os.path.exists
    assert not os.path.exists(new_file)
    with open(new_file, "w") as f:
        f.write("Hello from os_library_fn!\n")
    assert os.path.exists(new_file)

    # os.path.isfile
    assert os.path.isfile(new_file)
    assert not os.path.isfile(resources)

    # os.rename
    renamed_file: str = os.path.join(resources, "renamed_file.txt")
    os.rename(new_file, renamed_file)
    assert not os.path.exists(new_file)
    assert os.path.exists(renamed_file)

    # os.remove
    os.remove(renamed_file)
    assert not os.path.exists(renamed_file)

    # os.mkdir
    new_dir: str = os.path.join(resources, "new_dir")
    assert not os.path.exists(new_dir)
    os.mkdir(new_dir)
    assert os.path.exists(new_dir)

    # os.path.isdir
    assert os.path.isdir(new_dir)
    assert not os.path.isdir(renamed_file)

    # os.rmdir
    os.rmdir(new_dir)
    assert not os.path.exists(new_dir)

    # os.makedirs
    parent_dir: str = os.path.join(resources, "new_nested_dir")
    child_dir: str = os.path.join(parent_dir, "a")
    assert not os.path.exists(child_dir)
    os.makedirs(child_dir)
    assert os.path.exists(child_dir)
    os.rmdir(child_dir)
    os.rmdir(parent_dir)
    assert not os.path.exists(parent_dir)

    # os.path.dirname
    dir_name: str = os.path.dirname(child_dir)
    assert dir_name == parent_dir

    # os.path.basename
    base_name: str = os.path.basename(child_dir)
    assert base_name == "a"

    # os.path.abspath
    rel_path: str = os.path.join(resources, "..", "some_relative_path")
    abs_path: str = os.path.abspath(rel_path)
    assert abs_path.endswith("some_relative_path")

    # os.path.split
    split_path: tuple[str, str] = os.path.split(child_dir)
    assert tg(split_path, 0) == parent_dir
    assert tg(split_path, 1) == "a"

```

## Supported os functions

- `makedirs`
- `mkdir`
- `remove`
- `rmdir`
- `rename`

Note: certain arguments for these functions may not be supported.

## Supported os.path functions

- `join`
- `exists`
- `isdir`
- `isfile`
- `dirname`
- `basename`
- `split`
- `abspath`
