# Strings

Like Python strings.

## Declaration
Same as Python

```python
def pseudo_fn():
    # empty
    a: str = ""

    # some characters
    b: str = "hello"

    # repeating
    c: str = "abc" * 5

    # copy another string
    d: str = c

    # with single quotes
    e: str = 'hello'

    # f-strings
    f: str = f"{e}, world!\n{c}"
```

## Accessing an element

Same as Python

```python
def pseudo_fn(my_str: str):
    value: str = my_str[2]
```

## Accessing multiple elements

Same as Python with slices

```python
def pseudo_fn(my_str: str):
    a: str = my_str[1:4]
    b: str = my_str[1::2]
    c: str = my_str[::2]
    d: str = my_str[5:1:-1]
    # etc.

    # Or, with a slice object
    my_slice: slice = slice(0, 100, 2)
    e: str = my_str[my_slice]
```

## Methods

Not all of the Python string methods are supported in Py++ at the moment.

The supported methods are listed below and documented in the Python docs.

- `split`
- `join` (only works with lists for now)
- `replace`
- `startswith`
- `endswith`
- `count`
- `find`
- `index`
- `rindex`
- `strip`
- `lstrip`
- `rstrip`
- `upper`
- `lower`

## Other operations

Supported with the same behavior as Python:

- `in`, `not in`
- `+` (concatenation)
- `*` (repetition)
- `==`, `!=`
- `<`, `>`, `<=`, `>=`
- `len()`
- `min()`, `max()`
- `list()`
- `set()`
- `print()`

## Iteration support

Same as Python

```python
def pseudo_fn(characters: string):
    for character in characters:
        print(character)
```

## f-strings

Same as Python, but some things are not supported:

- Double or triple curly braces inside the f strings (i.e. `f"value: {{{value}}}"`)
- Format specificiations (e.g. `:.2f`, etc.)
- Conversion flags (i.e. `!r`, `!a`, `!s`)

## Raw strings

Same as Python (i.e. `r"this \n is a raw \t string"`).

## Triple-quoted strings

Same as Python 

```python
def pseudo_fn():
    a: str = """
line 1
line 2
line 3
"""
    print(a)
```

## Special characters

- `\n`
- `\t`
- `\r`
- `\b`
- `\f`
- `\\`

## Escaping

Like Python, escaping single and double quotes and special characters is supported with the backslash `\`
