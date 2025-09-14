# Resources

There is a built-in function `res_dir()` which returns a string for the path to the project resources dir. The resources dir is one of the 4 directories you should have in the root directory of your Py++ project. If you ran the `pypp init` script to create your project, then you will already have it.


## Usage

```python
from pypp_python import res_dir
import os


def pseudo_fn():
    resources_dir: str = res_dir()
    file: str = os.path.join(resources_dir, "text_file.txt")
    if os.path.isfile(file):
        print("text_file.txt file exists in resources dir")
```

If you have a `text_file.txt` in your resources directory, then if you had the above code and ran it, your file should be found. Make sure of the following so that your files are found correctly:

- When running with the Python interpreter, your virtual environment is located under `project_root/python`
- When running with the C++ generated executable, your **executable is under `project_root/cpp/build`**.
