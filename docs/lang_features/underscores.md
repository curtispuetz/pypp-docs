# Underscores

Underscores at the beginning of a name for certain things have a special meaning for the Py++ transpiler. They indicate that something is 'private'.

If you define a function (for example) in a src file starting with an underscore, then you should not import that function to some other file in your codebase. If you do, the C++ compiler will throw an error.

This is the case for the following:

- functions
- classes. I.e.:
    - normal classes
    - config classes
    - custom exceptions
    - interfaces
- variables (defined at the highest level of your Python module only)
- type aliases (defined at the highest level of your Python module only)

## What is this useful for?

If you define something as private, then you don't have to worry about namespace conflicts with the rest of your codebase. Additionally, it could make C++ compilation slightly faster.

## How does the Py++ transpiler do this?

When the Py++ transpiler sees one of these things starting with an underscore, it does not put the thing in the header file (only the .cpp file).
