# Transpiler Config Examples


## name_map.json

If you put a file `.pypp/transpiler_config/name_map.json` in your project:

```json
{
    "to_string": {
        "deque": {
            "to": "MyLibraryDequeImplementation"
        }
    }
}
```

then whenever you use the name `deque` in your code, instead of it being translated to `deque` in the C++ code like usual, it will be translated to `MyLibraryDequeImplementation`.

[api link](API.md/#name_mapjson)

## call_map.json

If you put a file `.pypp/transpiler_config/call_map.json` in your project:

```json
{
    "to_string": {
        "gl_gen_buffer": {
            "to": "myLibGlGenBufferImplementation"
        }
    }
}
```

then whenever you make a call to `gl_gen_buffer` in your code, instead of it being translated to `gl_gen_buffer` in the C++ code like usual, it will be translated to `libGlGenBufferImplementation`.

[api link](API.md/#call_mapjson)

## attr_map.json

If you put a file `.pypp/transpiler_config/attr_map.json` in your project:

```json
{
    "to_string": {
        "random.Random": {
            "to": "MyLibRandomImplementation",
        }
    }
}
```

then whenever you use the attribute `random.Random` in your code, instead of it being translated to `random.Random` in the C++ code like usual, it will be translated to `MyLibRandomImplementation`.

[api link](API.md/#attr_mapjson)

## always_pass_by_value.json

In Py++, only the [primitive types](../../lang_features/types/primitive_types.md) are always pass-by-value when used as function parameters or class data members. If you put a file `.pypp/transpiler_config/always_pass_by_value.json` in your project:

```json
{
    "GLuint": null
}
```

then `GLuint` will also always be pass-by-value.

[api link](API.md/#always_pass_by_valuejson)

## subscriptable_types.json

If you are using a type with a subscript (for example, `list[int]` is a type `list` with an `int` subscript), then the Py++ transpiler won't automatically translate the square brackets to angle brackets that we need in the generated C++ code. For a `list`, it will do it because the Py++ transpiler has that configured. But for other types to do it, the type must be listed in a `subscriptable_types.json` file.

If you put a file `.pypp/transpiler_config/subscriptable_types.json` in your project:

```json
{
    "MyCustomType": null
}
```

then the Py++ transpiler will translate `MyCustomType[int]` to `MyCustomType<int>`, instead of translating it to `MyCustomType[int]` (which will result in a failing C++ build).

I am thinking we can remove `subscriptable_types.json` in the future and have it done correctly automatically.

[api link](API.md/#subscriptable_typesjson)
