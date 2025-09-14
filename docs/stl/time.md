# Times

The Py++ time standard library is a little different, but still similar, to Python's time standard library. Also limited at the moment.

## Usage

```python
from pypp_python import time, auto


def pseudo_fn():
    start_time: auto = time.start()
    time.sleep(0.01)
    b: float = time.end(a)

    print(f"time.time() elapsed time: {b}")

    c: auto = time.perf_counter_start()
    time.sleep(0.01)
    d: float = time.perf_counter_end(c)

    print(f"performance time elapsed time: {d}")
```

For the `start()` and `perf_counter_start()` functions, you must specify the type as `auto`, and then you pass those variables only to `end()` and `perf_counter_end()` respectively.

## Supported functions

- `start`
- `end`
- `perf_counter_start`
- `perf_counter_end`
- `sleep`
