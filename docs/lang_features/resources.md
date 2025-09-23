# Resources

There is a built-in function `res_dir()` which returns a string for the path to the project resources directory. The resources directory located at `.pypp/resources`.


## Usage

```python
from pypp_python import os, res_dir


def pseudo_fn():
    resources_dir: str = res_dir()
    file: str = os.path.join(resources_dir, "text_file.txt")
    if os.path.isfile(file):
        print("text_file.txt file exists in resources dir")
```

If you have a `text_file.txt` in your resources directory, then if you had the above code and ran it, your file should be found. Make sure of the following so that your files are found correctly:

- When running with the Python interpreter, your virtual environment is located at the top level of your project (i.e. where the `.pypp` directory is)
- When running with the C++ generated executable, your **executable is under `.pypp/cpp/build`**.
